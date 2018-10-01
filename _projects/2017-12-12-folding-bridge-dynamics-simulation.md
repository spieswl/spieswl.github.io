---
layout:             project
title:              "Simulation of the Dynamics of a Folding Bridge"
date:               "2017-12-12"

description:        "Coding a simulation of the dynamics of the three-leaf, folding Hörn bridge in Kiel, Germany."
keywords:           dynamics, software, mathematica, simulation, bridge dynamics, euler-lagrange
tags:               [Dynamics, Simulation, Mathematica]

folders:
  images:           "horn-bridge-sim"                       # This path is project-dependent; don't forget to change it!

published:          true
---

Around the end of the **Fall 2017** term, I wrote a program in **[Wolfram Mathematica 11](https://www.wolfram.com/mathematica/)** with the intent to try and recreate the dynamics of the Hörn bridge in Kiel, Germany. This bridge is well-known for its elegant design and [unique opening and closing behavior](https://www.youtube.com/watch?v=E5BF3Lvmi_8), so I opted to try and recreate the bridge as a project in my Machine Dynamics class with [Todd Murphey](https://nxr.northwestern.edu/people/todd-murphey). Thanks to its status as a unique feat of bridge engineering, there was some technical literature available on the bridge which allowed me to gain some insight into the control and operation of the bridge. From there, it was all coding and evaluation in Mathematica.

<div class="project-image">
    <img src="{{ site.url }}/{{ site.project_assets }}/{{ page.folders.images }}/deploy.png" style="width:800px">
</div>

The Mathematica project notebook evolved through three different stages:

* **Initial problem setup & physical characterization**
* **Finding the correct constraints on the bridge's operation**
* **Modeling the impact condition and attempting some form of control over the bridge's action**

The final form of the project is an animation of the bridge unfolding under the effects of gravity and experiencing an impact on the far side pylon. I implemented some basic controls, mostly so that the framework would exist should I choose to return later and install a control law sufficient to safely deploy the bridge. As of right now, it can only delay (or accelerate) the opening of the bridge under gravity, which is not enough to prevent a hard collision on the far pylon. For the purposes of getting some meaningful data out of the simulation, I made sure to take a reading of the velocity experienced at the end of the final leaf at the moment of impact. At the moment, the simulated bridge hits far too hard to be considered "safe."

<div class="project-image">
    <img src="{{ site.url }}/{{ site.project_assets }}/{{ page.folders.images }}/results.png" style="width:940px">
</div>

This project, though I am admittedly not an avid Mathematica user, was an incredible amount of fun to work on. For those who would be interested in expanding on the foundations of the dynamic simulation already in place, or those curious to see an example of how I structured the evaluation and animation components, the source code for the project is available on **[GitHub](https://github.com/spieswl/horn-bridge-dynamics_sim)**! Finally, send me a message (check the **[About]({{site.url}}/about)** page for the administrator's contact information) if you have any comments or questions about the project!

**BONUS:** Here's a GIF of the animation in action.

<div class="project-image">
    <img src="{{ site.url }}/{{ site.project_assets }}/{{ page.folders.images }}/animation.gif" style="width:480px">
</div>



