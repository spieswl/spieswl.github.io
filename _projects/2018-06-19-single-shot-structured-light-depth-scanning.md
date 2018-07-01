---
layout:             project
title:              "UNDER DEVELOPMENT: High-Resolution 3D Reconstruction from a Single Capture Sequence"
date:               "2018-06-19"

description:        "UNDER DEVELOPMENT: This project, directed by Northwestern University's Computational Photography Lab, combines structured light projection, a stereo camera setup, and real-time image processing to perform extremely precise 3D reconstruction of near-field objects."
keywords:           structured light, 3d scanning, depth sensing, computer vision, stereo vision, rendering, point clouds, C++
tags:               [Computer Vision, Stereo Vision, 3D Modeling, Rendering, Point Clouds, C++, Under Development]

folders:
  images:           "highres-singleshot-3d-recon"           # This path is project-dependent; don't forget to change it!

published:          false
---

Over the summer months, I am participating in a research and development project that represents a particular fusion of stereo computer vision and 3D model reconstruction methods. As the lead software engineer, I will have the honor of being embedded in Northwestern University's **[Computational Photography Lab](http://compphotolab.northwestern.edu/)**. While part of the lab, I will be working with Dr. Oliver Cossairt and Dr. Florian Willomitzer on software implementing a novel application of structured light projection and subsequent measurement based on the captured image data. As the technology incorporates patented application techniques, I am not authorized to go into depth on the details of the project, but this article will direct you to related technology, talk about the project goals in abstract, and highlight skills I will be honing over the course of the next few months.

#### Related Work

For the purpose of understanding the technical context, watch this video on "_Flying Triangulation_" from the Institute of Optics, Information, and Photonics at the University of Erlangen-Nuremberg:

<div style="width: 100%; padding:8px 8px 8px 8px; text-align: center">
    <iframe width="560" height="400" src="https://www.youtube.com/embed/lspIIdSlXIk" frameborder="0" allow="autoplay; encrypted-media" allowfullscreen ></iframe>
</div>

The "_Flying Triangulation_" principle allows for freehand capture of dense, colorized, 3D data about objects within a measurement volume of roughly 0.4 cubic meters. Even within that measurement volume, local measurement uncertainty is near the sub-millimeter scale (<1.1 mm). In combination, this presents a model capture system that compares favorably with the capabilities of _every other active 3D scanning system on the market_.

I am working with Dr. Willomitzer and the **Computational Photography Lab** to develop the software controls and device interfaces for an _**even better**_ system.


#### High-Level Abstract

The idea is to make a system that can capture, generate, and render a high-resolution 3D point cloud of an object within the measurement volume with a single image capture sequence. This is achieved with captures from multiple machine vision cameras working in concert. My contribution is the development of a multithreaded application, programmed in C++, handling the following...

* _Physical interfacing with multiple machine vision cameras,_
* _Real-time processing of images from the aforementioned cameras (or offline sources),_
* _Projection calibration mechanisms,_
* _Generation of a dense, colorized point cloud,_
* and _Real-time rendering of the generated point cloud for inspection and eventual practical use._

Although this may not represent every library or package used, at a minimum I will be using features from the latest C++ standard (`C++17`), [OpenCV](https://opencv.org/), [Eigen](http://eigen.tuxfamily.org/index.php?title=Main_Page), [PCL](http://www.pointclouds.org/), an as-yet-unidentified rendering pipeline, and an as-yet-unidentified GUI framework to accomplish these tasks.

<img src="{{ site.url }}/{{ site.project_assets }}/{{ page.folders.images }}/01_technologies.png" style="width:800px; padding:4px 4px 4px 4px; display: block">


#### Honed Skills

As the leading (and only) software engineer on the project, my responsibilities include...

* _Requirements gathering,_
* _Software task planning,_
* _Consulting on hardware selection,_
* _Architecting and writing **all** code,_
* _Unit and Integration Testing,_
* _Documentation,_
* and, eventually, _Support of the final product._

While this seems like a tall order (and it is), the experience gained from taking part in such a project will be 
_**invaluable**_. Finally, pending successful development of the software, my experience working with the **Computational Photography Lab** and the deployment of such a technology should result in excellent networking opportunities as I look beyond my career at Northwestern University.

Since this project is in the _**Under Development**_ category, check back later this summer to see the results of our collaboration! Don't forget to send me a message (check the **[About](https://spieswl.github.io/about)** page) if you have any comments or questions about the project!