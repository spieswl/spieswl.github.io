---
layout:             feature
title:              "Driving a Kuka YouBot in simulation with Python"
date:               "2017-12-13"

description:        "I wrote a controller in Python to direct a mobile manipulator to follow a defined trajectory in a simulated environment."
keywords:           software, simulation, python, robotics, controls
tags:               [Python, Simulation, Robotics]

specifics:
    career:         false
    project:        true
    images:         "mobile-robot-follower"

published:          true
deadhead:           false
---

Northwestern University's robotics programs generally include significant exposure to robotic manipulation, otherwise understood as implementing kinematic and dynamic control of robotic systems.
By having access to exceptional faculty and serious facilities for manipulation research and development, aspiring roboticists are routinely given opportunities to tackle problems in robotic manipulation, motion planning, and controls.

One such example is the project featured here, as part of [Dr. Kevin Lynch](http://www.mccormick.northwestern.edu/research-faculty/directory/profiles/lynch-kevin.html)'s graduate-level Robotic Manipulation course.
Students were challenged to analyse the kinematics of a mobile robotic vehicle (the **Kuka YouBot**, a 5-axis manipulator mounted on a chassis with 4 mechanum wheels) and have it follow a pre-defined trajectory in a simulated environment.
The broader aims of the project involved demonstrating how we could apply our classwork to fulfilling a requirement that is very likely be encountered in a real robotic application.

First, some details on the challenge: The simulated problem was built with the help of Coppelia Robotics' **[VREP](http://www.coppeliarobotics.com/)** software.
A scenario containing a [Kuka YouBot](https://softroboticstoolkit.com/synergistic-design/testing/kuka) was constructed, and we were given a few different sets of initial conditions for the simulated mobile robot (position, orientation, and arm joint angles).
The goal was to use forward and inverse kinematics, motion planning, Python libraries for robotic manipulation, and an interface in the simulator to get the YouBot to follow a pre-defined end-effector trajectory over a five second timespan.
In order to successfully meet the requirements for this challenge, we needed to have implemented kinematic control of the 5-DOF robotic arm, use odometry to gauge and control the position and orientation of the mechanum-wheeled chassis, coordinate the motion of the arm and chassis to follow the desired trajectory simultaneously, and use PI control to correct for any deviation from the calculated path.

<div class="feature-image">
    <a href="{{ site.url }}/{{ site.assets.features }}/{{ page.specifics.images }}/01_youbot_home.png">
        <img src="{{ site.url }}/{{ site.assets.features }}/{{ page.specifics.images }}/01_youbot_home.png" width="480">
    </a>
</div>

The controller was written in Python, and uses the _**[numpy](http://www.numpy.org/)**_ and _**[Modern Robotics](https://github.com/NxRLab/ModernRobotics)**_ libraries to implement feed-forward, proportional, and proportional-integral control.
The _Modern Robotics_ library includes a number of highly useful functions designed to simplify the writing of manipulation and control code for arbitrary-constructed robotic systems.
For example, functions are available which can take a particular robot's screw axes at a given joint configuration and then return the Space or Body Jacobians for that configuration.
The controller can take the kinematic analysis results and perform calculations that will generate joint coordinates, chassis coordinates, and wheel coordinates that will put the robot on the path.
VREP would then parse a CSV-formatted list of time-varying joint angles and play back the resultant motion of the robot; an example of which is seen below.

<div class="feature-image">
    <a href="{{ site.url }}/{{ site.assets.features }}/{{ page.specifics.images }}/02_on-path_follower.gif">
        <img src="{{ site.url }}/{{ site.assets.features }}/{{ page.specifics.images }}/02_on-path_follower.gif" width="800">
    </a>
</div>

Results for the error feedback components are also included for both the "P-" and "PI"-controller configurations.
Note that the plots of position error shown below were taken from a scenario where the YouBot's initial conditions start the robot away from the desired path.

<div class="feature-image">
    <a href="{{ site.url }}/{{ site.assets.features }}/{{ page.specifics.images }}/03_P_ctrl_results.png">
        <img src="{{ site.url }}/{{ site.assets.features }}/{{ page.specifics.images }}/03_P_ctrl_results.png" width="540">
    </a>
    <a href="{{ site.url }}/{{ site.assets.features }}/{{ page.specifics.images }}/04_PI_ctrl_results.png">
        <img src="{{ site.url }}/{{ site.assets.features }}/{{ page.specifics.images }}/04_PI_ctrl_results.png" width="540">
    </a>
</div>

The end result is Python code that, when run, generates a motion plan that satisfies the challenge requirements.
With additional work, the code could also be ported to interact with a real YouBot controller and execute more sophisticated motion as part of a real system.
