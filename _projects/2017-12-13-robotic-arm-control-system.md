---
layout:             project
title:              "Driving a Kuka YouBot in VREP with Python"
date:               "2017-12-13"

description:        "I wrote a controller in Python to take a specific robotic arm, use its initial conditions, and follow a pre-defined trajectory through space."
keywords:           software, simulation, python, robotics, mobile robots, controls, controller, trajectory follower, youbot
tags:               [Python, Kinematics, Simulation, Robotics, Controls]

folders:
  images:           "mobile-robot-follower"                 # This path is project-dependent; don't forget to change it!

published:          true
---

Northwestern University's **MSR** program involves taking a number of sophisticated courses intended to familiarize students with advanced concepts in robotics, such as computer vision, robotic manipulation (understanding and implementing kinematic and dynamic control of robotic systems), embedded systems, and other areas. Part of the final evaluation of [Dr. Kevin Lynch](http://www.mccormick.northwestern.edu/research-faculty/directory/profiles/lynch-kevin.html)'s Robotic Manipulation course involved demonstrating the application of our accumulated knowledge about kinematic control to a problem involving a **Kuka YouBot** in an interactive simulation running in Coppelia Robotics' **[VREP](http://www.coppeliarobotics.com/)**.

We were given the initial conditions for the [Kuka YouBot](https://softroboticstoolkit.com/synergistic-design/testing/kuka) (position, orientation, and arm joint angles) and tasked with getting the YouBot to follow a pre-defined end-effector trajectory over a five second timespan. In order to successfully meet the requirements for this evaluation, we needed to have implemented kinematic control of the 5-DOF robotic arm, use odometry to gauge and control the position and orientation of the mechanum-wheeled chassis, and integrate a PI controller to take the robot's deviation from the desired path into account.

<div class="project-image">
    <img src="{{ site.url }}/{{ site.project_assets }}/{{ page.folders.images }}/youbot_home.png" style="width:480px">
</div>

The controller was entirely written in Python, and uses the _**[numpy](http://www.numpy.org/)**_ and _**[Modern Robotics](https://github.com/NxRLab/ModernRobotics)**_ libraries to implement feed-forward, proportional, and proportional-integral control. For those unfamiliar with the _Modern Robotics_ library, this library includes a number of highly useful functions designed to simplify the writing of manipulation and control code for arbitrary robotic systems. For example, functions are available which take a particular robot's screw axes at a given joint configuration and return the Space or Body Jacobians for that configuration. The controller takes the results of all these calculations over the entire time span and outputs each of the joint coordinates, chassis coordinates, and wheel coordinates to a CSV file. A particular scene available in VREP (with a YouBot in the center) can take the full list of time-varying joint angles and play back the resulting motion of the robot, seen below in the animated GIF.

<div class="project-image">
    <img src="{{ site.url }}/{{ site.project_assets }}/{{ page.folders.images }}/on-path_follower.gif" style="width:800px">
</div>


Results for the error feedback components are also included for both the "P-" and "PI"-controller configurations.

<div class="project-image">
    <img src="{{ site.url }}/{{ site.project_assets }}/{{ page.folders.images }}/P_ctrl_results.png" style="width:540px">
    <img src="{{ site.url }}/{{ site.project_assets }}/{{ page.folders.images }}/PI_ctrl_results.png" style="width:540px">
</div>

This code is not available at this moment for academic purposes; as far as I know, this project has been used multiple years in a row in Kevin's class. To shield future students from potential academic impropriety (*sorry, students!*), my code will not be made available on GitHub until Kevin comes up with a new final project! However, I can speak at length with those who have interests about the technical aspects of the code written for this project. As usual, check the **[About]({{site.url}}/about)** page for the relevant contact information if you want to chat!