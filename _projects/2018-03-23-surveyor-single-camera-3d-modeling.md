---
layout:             project
title:              "Surveyor: Single Camera 3D Modeling"
description:        "Independent computer vision project to build a point cloud modeling system that works exclusively with 2D images."
keywords:           ros, computer vision, slam, structure from motion, cameras, c++, python
tags:               [ROS, 3d Modeling, Computer Vision, Structure from Motion, C++, Python]

folders:
  images:           "surveyor" # This path is project-dependent; don't forget to change it!

published:          false
---

For the past several weeks, I have been working on an independent project that I feel has excellent potential. Nicknamed **Surveyor**, my project that blends computer vision and 3D reconstruction algorithms on a base provided by the Robot Operating System (ROS) into a software package that can take 2D images, perform some computation, and return a detailed point cloud. If it works correctly, the system could be used to model objects large and small, though my interest lies in being able to map ***spaces*** with some degree of accuracy. The true novelty of this project is that, unlike *most* systems involving visually driven 3D reconstruction, no separate depth sensing is performed at any step in the process.

Before I get much further, check out the **Surveyor** repository [over at GitHub](https://github.com/spieswl/surveyor). The software still has improvement to be done, but even now the project has promise.

When we were originally asked to submit project proposals to the MSR administrators, I knew I wanted a project that accomplished three specific things: **(1)** involved work in the field of computer vision, **(2)** gave me an opportunity to study code and perform a significant amount of coding myself, and **(3)** build something useful. The field of computer vision has an incredible amount of interesting work taking place, so novel project ideas are in high supply. Given how much recent work has been put into visual odometrical and photogrammetrical methods, I settled on the idea of making a mapping system with a single camera and no depth sensing. In practice, these systems already exist commercially, but often rely on tightly controlled lighting or the presence of fiducial markers in the field for references. I also wanted a lot of coding practice; any chance to improve my Python and C++, get familiar with best practices and the latest standards, and put a critical eye to other code were all highly valuable to me.

## Surveyor


## Why One Camera? Why No Depth Sensing?


## Results So Far...


## Future Work