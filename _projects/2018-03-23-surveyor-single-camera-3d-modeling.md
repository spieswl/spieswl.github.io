---
layout:             project
title:              "Surveyor: Single Camera 3D Modeling"
description:        "Independent computer vision project to build a point cloud modeling system that works exclusively with 2D images."
keywords:           ros, computer vision, slam, structure from motion, cameras, c++, python
tags:               [ROS, 3D Modeling, Computer Vision, Structure from Motion, C++, Python]

folders:
  images:           "surveyor"                              # This path is project-dependent; don't forget to change it!

published:          true
---

For the past several weeks, I have been working on an independent project that I feel has excellent potential. Nicknamed **Surveyor**, my project blends computer vision and 3D reconstruction algorithms on a base provided by the Robot Operating System (ROS) into a software package that can take 2D images, perform some computation, and return a dense point cloud. If it works correctly, the system could be used to model objects large and small, though my interest lies in being able to map ***spaces*** of arbitrary size with some degree of accuracy. The true novelty of this project is that, unlike *most* systems involving visually driven 3D reconstruction, no separate depth sensing is performed at any step in the process.

Before I get much further, check out the **Surveyor** repository [over at GitHub](https://github.com/spieswl/surveyor). The software still has improvement to be done, but even now the project has promise. More implementation and usage details are there, while this write-up is focused on the background history, thought processes, and lessons-learned from the project. I will also make mention of the two underlying algorithms (and accompanying code) that are being used: ***Direct Sparse Odometry (DSO)*** and  ***REgularized MOnocular Depth Estimation (REMODE)***, both somewhat recent developments in the computer vision domain. 

### Surveyor

<div class="project-video">
    <iframe src="https://drive.google.com/file/d/1PNMfx6Irwad7ekdQdnSYdrvyljGEk8MV/preview" allowFullscreen></iframe>
</div>

<br>

When we were originally asked to submit project proposals to the MSR administrators at Northwestern, I knew I wanted a project that accomplished three specific things: **(1)** involved work in the field of computer vision, **(2)** gave me an opportunity to study code and perform a significant amount of coding myself, and **(3)** build something useful. The field of computer vision has an incredible amount of interesting work taking place, so novel project ideas are in high supply. Given how much recent work has been put into visual odometrical and photogrammetrical methods, I settled on the idea of making a mapping system with a single camera and no depth sensing. In practice, systems like the one I built already exist commercially, but often rely on tightly controlled lighting or the presence of fiducial markers in the field for reference. I also wanted a lot of coding practice; any chances to improve my Python and C++, get familiar with best practices and latest standards, and put a critical eye to other code were all highly valuable to me. Finally, I wanted to build something for the program that could be (hopefully, would be) used by myself, my cohort-mates, or future grads coming through the program. Having contributed in only a limited fashion to the open source community, I wanted to build and release something that would be of value.

The initial design focused on the utilization of recent algorithms coming out of robotics and computer vision departments at the [Technical University of Munich](https://vision.in.tum.de/) (which you may know from the TUM Mono datasets, or other content like LSD-SLAM) and the [University of Zurich](http://rpg.ifi.uzh.ch/). Presentations of work from both groups have been and continue to be impressive in their own rights, but the original design idea seemed more interesting when structured as a fusion of approaches. The initial design progressed from the Output side and worked towards the Input side: `REMODE` was selected because it was a vetted dense reconstruction algorithm that only needs per-frame camera pose estimates (in addition to the granted 2D images), while `DSO` is lean, performs well over long trajectories, and can operate at high frame rates. That last aspect helps keep alive the long-term goal of having a mapping system that can operate untethered from a desktop (or even a laptop) computer. If more expensive calculation needs to take place to complete the `REMODE`-driven modeling step, such as parallel execution on a GPU, that can easily be offloaded to a later tethered "modeling" stage, after the initial captures and trajectory calculation is performed by `DSO` during the "mapping" stage.

### Why One Camera? Why No Depth Sensing?

A single camera system, though *potentially* more challenging for depth estimation tasks, simplifies hardware requirements and allowed me the chance to use monocular vision datasets. I spent a majority of time in development and testing working with image sequences from a Fujifilm X-T2 mirrorless camera; not typical computer vision capture equipment to be sure! Single camera systems are also far more common in the world; I think there certainly exists a future where someone could pick up a smartphone with a well-characterized optical system, record some images or video, offload some calculation, and get a 3D model out of it. 

No depth sensing seemed like a good way to differentiate this approach. [Google Cartographer](https://google-cartographer.readthedocs.io/en/latest/), as one example, is a highly sophisticated SLAM system that relies on multiple inputs, including laser- or IR-based depth sensing. While this software is nowhere near as mature as Cartographer, there are far fewer moving parts to deal with here. Unfortunately, this leads to the imaging component being a true "single point of failure". There are no backups to **Surveyor** in situations where conditions are not conducive to taking good pictures.

### Results So Far...

Below are a select few videos of importance, minus the one I made in a professional vein shown prior in this write-up, that show the intermediate and final results of the **Surveyor** system up to this point. There is still work to be done on the software side, and I have a growing list of hardware and technical considerations to improve the user experience concerning this application that I intend to publish as an update to this post.

- This video is pretty close to **Surveyor** working exactly as desired. This video shows the results of around 370 still images *(I refer to them as snaps to disambiguate from video frames)*, with 900x600 resolution. These images were down-sampled from 6000x4000 resolution captures on the Fujifilm X-T2. Note that the point cloud model starts out in pretty good shape, but right around 0:33 on the timer, significant noise gets introduced into the result. 

<div class="project-video">
    <iframe src="https://drive.google.com/file/d/1i3VFRosg8Wgpp74V4YPQ8L_vGL_gnX_z/preview" allowFullScreen></iframe>
</div>

<br>

- This next video is of a clear failure. This video shows the results of **Surveyor** processing a ~4000-frame video (AVI format) taken at 1280x720 resolution at 60 frames/sec. I visually inspected the video, and I thought it was smooth as silk. I could not clearly pick out ANY distortion, tearing, blurring, or anything else that might cause a problem. Sure enough...watch what happens to the dense point cloud throughout the video.

<div class="project-video">
    <iframe src="https://drive.google.com/file/d/1vPdYIB3mlwVFP6zwow8iLrNSvJpmTIaW/preview" allowFullScreen></iframe>
</div>

<br>

- Here is a quick video showing some of the intermediate results from using DSO. This is also running on the 370 snaps down-sampled to 900x600 from the X-T2.

<div class="project-video">
    <iframe src="https://drive.google.com/file/d/1UcqqPfop-trrudnOrNwVeff2xeKAHdZ_/preview" allowFullScreen></iframe>
</div>

<br>

- This video is also of the intermediate results from using DSO. This video was taken in the middle of a run using the ~4000-frame video feed at 60 frames/sec. The errors here are far more subdued than what can be seen in the REMODE video, though towards the end you can see duplication and skewing in the sparse point cloud.

<div class="project-video">
    <iframe src="https://drive.google.com/file/d/1U-MNpBcBBD8gcgmZU77JqgXbr0TwR1yn/preview" allowFullScreen></iframe>
</div>

<br>

In any case, the results show a mix of very promising results mixed with some very odd failures. The video feed, at least from the X-T2 camera (regardless of video quality, it seems), should be avoided at all costs, while the snapshots yield the most promising results.

<!--
<TODO : ## Lessons Learned?>
<TODO : ## Future Work>
-->