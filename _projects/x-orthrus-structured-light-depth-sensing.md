---
layout:             project
title:              "Orthrus: High-Resolution 3D Reconstruction from a Single Shot"
date:               "2018-06-19"

description:        "This project, directed by Northwestern University's Computational Photography Lab, combines structured light projection, a stereo camera setup, and real-time image processing to perform extremely precise 3D reconstruction of near-field objects."
keywords:           structured light, 3d scanning, depth sensing, computer vision, stereo vision, rendering, point clouds, C++, opencv
tags:               [C++, OpenCV, Computer Vision, Stereo Vision, 3D Modeling, Rendering, Point Clouds, Under Development]

specifics:
    featured:       true
    images:         "orthrus"

published:          false
---

Over the summer months, I am participating in a research and development project that represents a particular fusion of stereo computer vision and 3D model reconstruction methods. As the lead software engineer, I will have the honor of being embedded in Northwestern University's **[Computational Photography Lab](http://compphotolab.northwestern.edu/)** for the duration of the project. While part of the lab, I will be working with Dr. Oliver Cossairt and Dr. Florian Willomitzer on software implementing a novel application of structured light projection and subsequent measurement based on the captured image data. I will be going light on the details of the project in this write-up, but the **Related Work** section will direct you to an article written by Dr. Willomitzer on the subject, and I will talk about the project goals in abstract and highlight skills I will be honing over the course of the next few months.

#### Related Work

For the purpose of understanding the technical context, watch the linked video on the novel, optical 3D sensing principle at work, originally demonstrated by the Osmin group at the University of Erlangen-Nuremberg. For more information on their development, refer to [Single-shot 3d motion picture camera with a dense point cloud by Willomitzer and Häusler](https://www.osapublishing.org/oe/abstract.cfm?uri=oe-25-19-23451), published on Optics Express.

<div style="width: 100%; padding:8px 8px 8px 8px; text-align: center">
    <iframe width="560" height="315" src="https://www.youtube.com/embed/hlMLigk1UfU?rel=0" frameborder="0" allow="autoplay; encrypted-media" allowfullscreen></iframe>
</div>

This multi-line triangulation principle allows for the efficient capture of dense 3D data about observed objects with the use of only two cameras. Even with minimal hardware and "single-shot" operation, local measurement uncertainty is better than 200 micrometers. The system works without needing spatial encoding or other indexing methods that might cause the loss of spatial resolution. All of this results in an efficient, capable model capture system that compares favorably with the capabilities of _every other active 3D scanning system on the market_.

I am working with Dr. Willomitzer and the **Computational Photography Lab** to develop the real-time software controls and device interfaces for the 3D sensing system described in the the multi-line triangulation article.


#### High-Level Abstract

The idea is to make a system that can capture, generate, and render a high-resolution 3D point cloud of an object within the field with a single image capture sequence. This is achieved with captures from multiple machine vision cameras working in tandem. My contribution is the development of a multithreaded application, programmed in C++, handling the following...

* _Physical interfacing with multiple machine vision cameras,_
* _Real-time processing of images from the aforementioned cameras (or offline sources),_
* _Projection calibration mechanisms,_
* _Generation of a dense, colorized point cloud,_
* and _Real-time rendering of the generated point cloud for inspection and eventual practical use._

Although this may not represent every library or package used, at a minimum I will be using features from the latest C++ standard (`C++17`), [OpenCV](https://opencv.org/), [Eigen](http://eigen.tuxfamily.org/index.php?title=Main_Page), [PCL](http://www.pointclouds.org/), an as-yet-unidentified rendering pipeline, and an as-yet-unidentified GUI framework to accomplish these tasks.

<div class="project-image">
    <img src="{{ site.url }}/{{ site.assets.projects }}/{{ page.specifics.images }}/01_technologies.png" style="width:800px">
</div>

We have also talked at length about potential feature expansions, which could add mesh generation, Python bindings, or other quality-of-life improvements.


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
