---
layout:             project
title:              "Fusing Images and Depth Maps with CUDA"
date:               "2018-03-29"

description:        "A small parallel programming project, acting as a base for future CUDA development, which serves to fuse 2D images and depth maps into a PCD (Point Cloud) file."
keywords:           CUDA, computer vision, parallel computing, point clouds, c++
tags:               [C++, CUDA, Computer Vision, Parallel Programming, Point Clouds]

folders:
  images:           "CUDA-2d-depth-fusion"                  # This path is project-dependent; don't forget to change it!

published:          true
---

<div class="project-image">
    <img src="{{ site.url }}/{{ site.project_assets }}/{{ page.folders.images }}/03_transfer_process.png" style="width:800px">
</div>

I put together a small CUDA program at the end of March 2018 to supplement a class I was taking on parallel computing with [Nikos Hardavellas](http://users.eecs.northwestern.edu/~hardav/). At the time, I was neck deep in my **[Surveyor]({{site.url}}/projects/2018/surveyor-single-camera-3d-modeling)** project, which involved a lot of Point Cloud generation and wanted an excuse to develop some GPU code that might end up having a future use. I ended up writing some `C++` / `CUDA` code that achieved a few specific goals:

1. Create a small base off which I could quickly create move in-depth, parallelized computer vision code in the future.
2. Experiment with methods to time host-to-device transfers, kernel execution, and device-to-host transfers.
3. Combine data from multiple different sources in a host program to return a third, fused output.

The last goal could be specifically outlined as **take an input 2D image, fuse it with known depth information from other sources, and pack the point vectors into a Point Cloud format file (PCD).**

While the skeleton code I put together achieves **Goal 3** with a fairly simple kernel (_shown below_), this code contains some boilerplate features: block / thread indexing, padding input data to neatly fit in shared device memory, use of constant (_texture_) memory, and some bitwise operations that can be easily changed out later.

```cpp
__global__ void convertImgToPCD(unsigned char* image, float* depth, float* PCD, size_t img_rows, size_t img_cols)
{
    int index = blockIdx.x * blockDim.x + threadIdx.x;

    // Shared and constant memory assignment
    __shared__ unsigned char sh_IMG[1024];
    __shared__ float sh_DEP[256];

    // Phase 1 : Copy global memory into shared memory
    sh_IMG[4*threadIdx.x] = image[4*index];                     // B Channel
    sh_IMG[4*threadIdx.x+1] = image[4*index+1];                 // G Channel
    sh_IMG[4*threadIdx.x+2] = image[4*index+2];                 // R Channel
    sh_IMG[4*threadIdx.x+3] = image[4*index+3];                 // Padding from host (CAN BE DISCARDED)

    sh_DEP[threadIdx.x] = depth[index];                         // Depth value

    __syncthreads();

    // Phase 2 : Perform some computation on the pixels
    // TODO: Replace Phase 2 code with real code in future applications (essentially placeholder ops for now)
    float x, y, z, rgb;

    x = ((float) (index % img_cols)) + TxMat[3];
    y = ((float) (floorf(index / img_cols))) + TxMat[7];
    z = (sh_DEP[threadIdx.x] + TxMat[11]);
    rgb = (sh_IMG[4*threadIdx.x+2]) << 16 | (sh_IMG[4*threadIdx.x+1]) << 8 | (sh_IMG[4*threadIdx.x]);

    __syncthreads();

    // Phase 3 : Move the new data back into the output matrix (ready for packing into a PCD on the host)
    PCD[4*index] = x;
    PCD[4*index+1] = y;
    PCD[4*index+2] = z;
    PCD[4*index+3] = rgb;

    __syncthreads();
}
```

The results are mostly for show, as I ran this code on the images and depthmaps from Handa, Newcombe, Angeli, and Davison **[1]**, which are fully synthetic. See here:

<div class="project-image">
    <img src="{{ site.url }}/{{ site.project_assets }}/{{ page.folders.images }}/01_image_results.png" style="width:1500px">
</div>

In addition, an easy way to get profiling information for **Goal 2**, especially about host->device and device->host transfer times, was really critical in looking at whether or not it made sense (from an execution timing perspective) to involve the GPU at all. Turns out, based on results from my machine (`Core i7-7700k and a GeForce GTX 980 Ti, Compute 5.2`), that execution time for smaller inputs tends to be dominated by GPU transfer overhead.

<div class="project-image">
    <img src="{{ site.url }}/{{ site.project_assets }}/{{ page.folders.images }}/02_transfer_results.png" style="width:900px">
</div>

If you recall that my kernel code posted above is just a few FP operations to leave room for future expansion, so I could quickly shift the balance back into the GPU's favor by doing more sophisticated operations in the kernel. The neatest feature I picked up on (which enabled measurement of the prior transfers), thanks to [Mark Harris' article on CUDA events](https://devblogs.nvidia.com/how-implement-performance-metrics-cuda-cc/), was using built-in timing capabilities in the CUDA events API. An example of which can be viewed below:

```cpp
#define CUDA_CALL(x) { cudaError_t cuda_error__ = (x); if (cuda_error__) printf("CUDA error: " #x " returned \"%s\"\n", cudaGetErrorString(cuda_error__)); }

...

void xferDataFromHostToDevice(void* device_ptr, const void* host_ptr, size_t data_size)
{
    cudaEvent_t start, stop;
    cudaEventCreate(&start);
    cudaEventCreate(&stop);

    cudaEventRecord(start);
    CUDA_CALL(cudaMemcpy(device_ptr, host_ptr, data_size, cudaMemcpyHostToDevice));
    cudaEventRecord(stop);

    // PROGRAMMER NOTE : PROFILING CODE BELOW
    cudaEventSynchronize(stop);

    float mSec = 0.0;
    cudaEventElapsedTime(&mSec, start, stop);

    printf("GPU Memory Transfer Time (to Device) (in ms) : %f \n", mSec);
}
```

As for **Goal 1**, I have used successfully used code that _**started**_ from this project, although it has morphed so much as to be nigh unrecognizable except for some utility functions and the data transfer functions executed on the host. The full, original `main` function I started with is shown so you readers can get a sense of what the base program was supposed to be doing.

```cpp
int main(int argc, char **argv)
{
    // Initial device checks
    if (!(checkCudaFunctionality()))
    {
        return 5;
    }

    // Get file path for images and depthmaps
    std::string image_file = argv[1];
    std::string depthmap_file = argv[2];
    std::string pose_file = argv[3];

    ////////////////////////////////////////////////////////////
    // Parsing image file, saving to data structure
    cv::Mat input_image = cv::imread(image_file);

    // Quick image properties
    size_t img_height = input_image.rows;
    size_t img_width = input_image.cols;
    size_t img_size = img_height * img_width;

    unsigned char * img_values = input_image.data;
    ////////////////////////////////////////////////////////////

    ////////////////////////////////////////////////////////////
    // Parsing depth map file, saving to data structure
    float * DM_values = new float[img_size];

    std::ifstream depth_reader(depthmap_file);

    for (int k = 0; k < img_size; ++k)
    {
        depth_reader >> DM_values[k];
    }
    ////////////////////////////////////////////////////////////

    ////////////////////////////////////////////////////////////
    // Parsing pose (transform) file, saving to data structure
    float * pose_values = new float[TRANSFORM_MAT_SIZE + 1];

    std::ifstream pose_reader(pose_file);

    // Ignore the frame ID (first value) for our purposes, rest are row major Transformation matrix
    for (int m = 0; m < TRANSFORM_MAT_SIZE+1; ++m)
    {
        pose_reader >> pose_values[m];
    }
    ////////////////////////////////////////////////////////////

    // Pad the needed memory of the image color channels to align on the GPU
    // Allocate device memory for the input image
    // Transfer data to the device
    size_t padded_img_size = img_size * 4 * sizeof(char);
    unsigned char * padded_img = new unsigned char[img_size * 4];
    unsigned char * padded_img_dev;

    padded_img_dev = (unsigned char*) allocateDeviceMemory(padded_img_size);
    padImageData(padded_img, img_values, img_size);

    xferDataFromHostToDevice(padded_img_dev, padded_img, padded_img_size);

    // Allocate device memory for depth map
    size_t DM_size = img_size * sizeof(float);
    float * DM_dev;

    DM_dev = (float*) allocateDeviceMemory(DM_size);
    xferDataFromHostToDevice(DM_dev, DM_values, DM_size);

    // Allocate constant device memory for transformation matrix - NOTE: Pointer not passed to kernel function invocation
    size_t T_size = TRANSFORM_MAT_SIZE * sizeof(float);

    xferConstToDevice(pose_values+1, T_size);       // Passing the pointer to the first element of the T matrix

    // Allocate device memory for PCD output file
    size_t PCD_size = img_size * 4 * sizeof(float);
    float * PCD_values = new float[img_size * 4];
    float * PCD_dev;

    PCD_dev = (float*) allocateDeviceMemory(PCD_size);

    // Execute device kernels to work on data and convert to point cloud
    initPppdeCode(padded_img_dev, DM_dev, PCD_dev, img_size, img_height, img_width);

    // Extract point cloud information from device
    xferDataFromDeviceToHost(PCD_values, PCD_dev, PCD_size);

    // Save point cloud information to disk
    std::ofstream PCD_writer;
    PCD_writer.open("output.PCD", std::ios::out);

    // Manually assembling the headers, since this is just prototype code
    PCD_writer << "VERSION .7\n" \
               << "FIELDS x y z rgb\n" \
               << "SIZE 4 4 4 4\n" \
               << "TYPE F F F F\n" \
               << "COUNT 1 1 1 1\n" \
               << "WIDTH " << img_width << "\n" \
               << "HEIGHT " << img_height << "\n" \
               << "VIEWPOINT 0 0 0 1 0 0 0\n" \
               << "POINTS " << img_size << "\n" \
               << "DATA ascii\n";

    for (int n = 0; n < img_size; ++n)
    {
        PCD_writer << PCD_values[4*n] << " ";
        PCD_writer << PCD_values[4*n+1] << " ";
        PCD_writer << PCD_values[4*n+2] << " ";
        PCD_writer << PCD_values[4*n+3] << " ";
        PCD_writer << "\n";
    }

    PCD_writer.close();

    // Clean up memory
    deallocateDeviceMemory(PCD_dev);
    deallocateDeviceMemory(DM_dev);
    deallocateDeviceMemory(padded_img_dev);
    delete[] PCD_values;
    delete[] padded_img;
    delete[] pose_values;
    delete[] DM_values;

    return 0;
}
```

That's all for this mini-project write-up. As usual, check the **[About]({{site.url}}/about)** page and reach out to the Administrator email if you have any questions or comments!

#### References

**[1]**  [A. Handa, R. Newcombe, A. Angeli, and A. Davison, “Real-time camera tracking: When is high frame-rate best?” in _Proc. IEEE European Conf. on Computer Vision_, 2012](https://link.springer.com/chapter/10.1007/978-3-642-33786-4_17)
