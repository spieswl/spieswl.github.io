---
layout:             project
title:              "Using Rethink Robotics' Baxter to Open Lidded Containers"
description:        "Rethink Robotics' Baxter robot finds all kinds of use in research environments. We use Baxter to find the lid of a bottle (capped with an AR tag) and initiate a sequence to open the bottle."
keywords:           ros, python, baxter, object manipulation, rethink robotics, AR markers
tags:               [ROS, Python, Baxter, Object Manipulation]

folders:
  images:           "baxter-container"                      # This path is project-dependent; don't forget to change it!

published:          true
---


We have a few Baxter and Sawyer robots from Rethink Robotics down in the labs at Northwestern, so we students often get the chance to write code or fabricate parts for 'mature' robots when presented with practical problems. In the interests of giving us hands-on time with these robots, several student groups were asked to come up with mini-projects involving either Baxter or Sawyer. These mini-projects needed to have some defined end goal that could be achieved in the span of only two weeks with a team of 4-5 individuals. Proposed mini-projects ranged from manipulating Keurig K-Cups to make coffee, solving a [Labyrinth](https://en.wikipedia.org/wiki/Labyrinth_(marble_game)), integrating a voice control system, and opening screw-lid containers with Baxter or Sawyer's end effectors.

Our project team, composed of myself and four others hailing from the *MS Robotics*, *MS MechEng*, or *NXR* departments, proposed a broad project that would program one of the robots to recognize a lidded object, move to a pounce position over that object, grasp the lid, set the lid on the table, and reverse that process to reattach the lid. Our target object was an empty bottle of *Tide* detergent, which has clear coloring and a nicely defined lid with strong threads and a large grasping area. Originally, we thought the most interesting version of this project involved performing some object recognition, using the integrated cameras in Baxter, and then determining where the lid must be. Whether or not the lid could be grasped with the current gripper configuration, where Baxter might need to position its hand, and detection of the table-top surface to set the lid down were all other proposed ideas. Control over all of these components would be handled by a state machine, with state detection facilitated by, among other things, the joint and actuator sensors in Baxter. However, with only two weeks to plan, develop, test, and refine our project before presentation, much of that had to be (regrettably) scrapped. We settled upon tracking an AR marker to determine the pose of the lid (and, indirectly, the rest of the bottle), using Baxter's integrated Inverse Kinematics (IK) solver to maneuver around the *Tide* bottle, and building a canned sequence that Baxter would follow.

Baxter natively runs the Robot Operating System, so all of our code was written in Python, designed to run on ROS, and makes use of other "ROS-wrapped" libraries. In particular, AR tag detection was facilitated by the [**ar_track_alvar**](http://wiki.ros.org/ar_track_alvar) package. Otherwise, all code was written by our team, and can be found [here, on fellow MSR student Ahalya Mandana's GitHub](https://github.com/am2512/baxter_final_project) or [here, a project fork on my GitHub](https://github.com/spieswl/container_manipulator) that is dedicated to continuing experimentation and further post-project clean-up.

<div style="width: 100%; text-align: center">
    <img src="{{ site.url }}/{{ site.project_assets }}/{{ page.folders.images }}/opening_lid.gif" style="width:308px; padding:8px 24px 8px 24px;">
    <img src="{{ site.url }}/{{ site.project_assets }}/{{ page.folders.images }}/move_bottle.gif" style="width:370px; padding:8px 24px 8px 24px;">
</div>

Some of our results can be found immediately prior to and after this section. The video does a particularly good job of demonstrating Baxter's capabilities as a laundry assistant. We cleverly surmised that the additional joint range of Baxter's wrists (versus that of a human) could make it possible to remove the lid in one full counter-clockwise motion. Keep an eye out for the small reverse turn you see right before the tightening step; that was a quick tweak we made to see if we can get the threads seated correctly every time Baxter brings the lid down, and it works...just like how a person would backthread a bottle!

<div class="project-video">
    <iframe src="https://drive.google.com/file/d/1EaoCfeFKYYQqXFVEPJns-Yj-1wORPHBg/preview" allowFullscreen></iframe>
</div>

<br>

On the last note, I have been experimenting with different decision making structures (see [decision_making](http://wiki.ros.org/decision_making) on the ROS Wiki) that will eventually get rolled into the `container_manipulator` package to replace the canned sequence routine contained in `sequencer.py`, but those updates are not quite ready. Come back later for more project updates, or check out the GitHub repository for a more up-to-date idea of when the project code has been improved.