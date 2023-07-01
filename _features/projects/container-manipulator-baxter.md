---
layout:             feature
title:              "Using a Baxter Robot to Open Containers with Lids"
date:               "2017-12-17"

description:        "Rethink Robotics' Baxter finds all kinds of use in research environments. Fellow members of my MS cohort and I used Baxter to find the lid of a Tide jug (topped with an AR tag) and initiate a set of motions and gripper controls to unseal and reseal the container."
keywords:           ros, python, baxter, object manipulation, rethink robotics, AR tags
tags:               [ROS, Python, Robotics]

specifics:
    career:         false
    project:        true
    images:         "baxter-container"

published:          true
deadhead:           false
---

The robotics labs at Northwestern possess a few research platforms that the students occasionally work with.
A few such research robots are from the _Rethink Robotics_ family, **[Baxter](https://www.rethinkrobotics.com/baxter/)** and **[Sawyer](https://www.rethinkrobotics.com/sawyer/)**.
At the end of our primer class on ROS & Embedded Systems, the students divided into groups and each group was asked to come up with project ideas that were based upon the use of either of those robots.
These projects needed to have some defined end goal that could be achieved in the span of two weeks with a team of 4 to 5 individuals.
Proposed projects ranged from manipulating Keurig K-Cups to make coffee, solving a _[Labyrinth](https://en.wikipedia.org/wiki/Labyrinth_(marble_game))_, integrating a voice control system, and opening screw-lid containers with Baxter or Sawyer's end effectors.

The team I was a member of was composed of myself and four others hailing from the _MS Robotics_, _MS MechEng_, and _NXR_ groups.
We proposed a project that involved programming _Baxter_ to recognize a lidded object, move to a relatively-defined "pounce" position over that object, grasp the lid, set the lid on the table, and reverse that process to reattach the lid.
Our target object was an empty jug of **_Tide_** detergent, which has solid, consistent coloring and a lid with strong threads and a large area for grasping. We knew that the most interesting version of this project involved full end-to-end object recognition, pose determination, and lid manipulation in a semi-controlled space.
In fact, all of those concepts are theoretically possible with _Baxter_; the integrated cameras in its arms could be used for object recognition, along with pose estimation by backsolving where the lid should be on our target container.
Other proposed ideas were determining whether or not the lid could be grasped with the current gripper configuration, solving for where Baxter would need to position its hand on a particular lid, and planar segmentation of the table surface to determine where it could safely set the lid down.
However, with only two weeks to plan, develop, test, and refine our project before the presentation, many of those ideas had to be scrapped.
We settled on tracking an AR tag to determine the pose of the lid (_and, indirectly, the rest of the bottle_), using Baxter's integrated inverse kinematics (IK) solver to maneuver around the **_Tide_** jug, and programming a set of motions that Baxter would execute to open and close the lid once it was in position to do so.
Control over all of these behaviors would be handled by a state-machine-like Python script, with control and state estimation facilitated by joint and actuator sensors in Baxter's arms and end effectors.

Baxter natively runs **ROS**, so all of our code was written in Python, was linked to Baxter's `rosmaster`, and enabled us to use other ROS packages as desired.
In particular, AR tag detection was facilitated by the **[ar_track_alvar](http://wiki.ros.org/ar_track_alvar)** package.
Otherwise, all code was written by our team over the course of a two-week period.

<hr>

##### Results

<div class="feature-image">
    <img src="{{ site.url }}/{{ site.assets.features }}/{{ page.specifics.images }}/01_opening_lid.gif" width="308">
    <img src="{{ site.url }}/{{ site.assets.features }}/{{ page.specifics.images }}/02_move_bottle.gif" width="382.013">
</div>

The video below does a nice job of demonstrating Baxter's latent capabilities as a laundry assistant.
We cleverly used the additional joint range of Baxter's wrists (_versus that of a human wrist_) to remove the lid in one full counter-clockwise motion.
Keep an eye out for the small reverse turn you see right before the tightening step; that was a tweak we added to try seating the threads correctly when Baxter brings the lid down.
That motion works in the same way a person might backthread a lid before tightening it down.

<div class="feature-video">
    <iframe src="https://player.vimeo.com/video/246549829" frameborder="0" allow="fullscreen" allowfullscreen></iframe>
</div>

The source code for this project can be found [on my GitHub](https://github.com/spieswl/container_manipulator).
I forked and archived the project so it would exist past the end of the class, although code quality is lacking and _Baxter_ itself became a defunct platform after Rethink was acquired.
As it stands, this project now largely serves as a museum piece, so be forewarned if using this as a reference.
