---
layout:             project
title:              "Using Rethink Robotics' Baxter to Open Lidded Containers"
date:               "2017-12-17"

description:        "Rethink Robotics' Baxter robot finds all kinds of use in research environments. We use Baxter to find the lid of a bottle (capped with an AR tag) and initiate a sequence to open the bottle."
keywords:           ros, python, baxter, object manipulation, rethink robotics, AR markers
tags:               [ROS, Python, Robotics, Baxter]

folders:
  images:           "baxter-container"                      # This path is project-dependent; don't forget to change it!

published:          true
---

We have a few Baxter and Sawyer robots from Rethink Robotics down in the labs at Northwestern, so we students often get the chance to write code or fabricate parts for 'mature' robots when presented with practical problems. In the interests of giving us hands-on time with these robots, several student groups were asked to come up with mini-projects involving either **[Baxter](https://www.rethinkrobotics.com/baxter/)** or **[Sawyer](https://www.rethinkrobotics.com/sawyer/)**. These mini-projects needed to have some defined end goal that could be achieved in the span of only two weeks with a team of 4-5 individuals. Proposed mini-projects ranged from manipulating Keurig K-Cups to make coffee, solving a _[Labyrinth](https://en.wikipedia.org/wiki/Labyrinth_(marble_game))_, integrating a voice control system, and opening screw-lid containers with Baxter or Sawyer's end effectors.

Our project team, composed of myself and four others hailing from the _MS Robotics_, _MS MechEng_, or _NXR_ groups, proposed a project that would program one of the robots to recognize a lidded object, move to a pounce position over that object, grasp the lid, set the lid on the table, and reverse that process to reattach the lid. Our target object was an empty bottle of _**Tide**_ detergent, which has clear coloring and a nicely defined lid with strong threads and a large grasping area. Originally, we thought the most interesting version of this project involved performing some object recognition, using the integrated cameras in Baxter, and then determining where the lid must be. Whether or not the lid could be grasped with the current gripper configuration, where Baxter might need to position its hand, and detection of the table-top surface to set the lid down were all other proposed ideas. Control over all of these components would be handled by a state machine, with state detection facilitated by, among other things, the joint and actuator sensors in Baxter. However, with only two weeks to plan, develop, test, and refine our project before presentation, much of that had to be (regrettably) scrapped. We settled upon tracking an AR marker to determine the pose of the lid (and, indirectly, the rest of the bottle), using Baxter's integrated Inverse Kinematics (IK) solver to maneuver around the _**Tide**_ bottle, and building a canned sequence that Baxter would follow.

Baxter natively runs the Robot Operating System, so all of our code was written in Python, designed to run on ROS, and makes use of other "ROS-wrapped" libraries. In particular, AR tag detection was facilitated by the **[ar_track_alvar](http://wiki.ros.org/ar_track_alvar)** package. Otherwise, all code was written by our team over the course of a two-week period.

<div class="project-image">
    <img src="{{ site.url }}/{{ site.project_assets }}/{{ page.folders.images }}/opening_lid.gif" style="width:308px; padding:8px 8px 8px 8px;">
    <img src="{{ site.url }}/{{ site.project_assets }}/{{ page.folders.images }}/move_bottle.gif" style="width:370px; padding:8px 8px 12px 8px;">
</div>

Some of our results can be found immediately prior to and after this section. The video does a particularly good job of demonstrating Baxter's capabilities as a laundry assistant. We cleverly surmised that the additional joint range of Baxter's wrists (versus that of a human) could make it possible to remove the lid in one full counter-clockwise motion. Keep an eye out for the small reverse turn you see right before the tightening step; that was a tweak we made to see if we can get the threads seated correctly every time Baxter brings the lid down, and it works...just like how a person might backthread a bottle!

<div class="project-video">
    <iframe src="https://drive.google.com/file/d/1EaoCfeFKYYQqXFVEPJns-Yj-1wORPHBg/preview" allowFullscreen></iframe>
</div>

The source code for this project can be found [here, on my GitHub](https://github.com/spieswl/container_manipulator); note that this fork is dedicated to continuing experimentation and further post-project clean-up. Otherwise, forked commit is exactly as we left it at the end of the demonstration.