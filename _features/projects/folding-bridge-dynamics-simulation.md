---
layout:             feature
title:              "Simulation of the Dynamics of a Folding Bridge"
date:               "2017-12-12"

description:        "Coding up a simulation of the dynamics of the three-leaf, folding Hörn bridge in Kiel, Germany."
keywords:           dynamics, software, mathematica, simulation, mathematica
tags:               [Dynamics, Simulation]

specifics:
    career:         false
    project:        true
    images:         "horn-bridge-sim"

published:          true
deadhead:           false
---

This is a cool example of the comparisons that can be made between real and simulated projects.
During the **Fall 2017** term at Northwestern, I was taking a Machine Dynamics class with [Todd Murphey](https://nxr.northwestern.edu/people/todd-murphey).
Among many other things, the behavior of dynamic systems under the influence of constraints was a core focus of our classwork.
For the class' final project, I put together a **[Wolfram Mathematica](https://www.wolfram.com/mathematica/)** workbook that recreates the dynamic behavior of the Hörn bridge in Kiel, Germany.
This bridge is well-known for its elegant design and [unique opening and closing behavior](https://www.youtube.com/watch?v=E5BF3Lvmi_8), so I attempted to mathematically capture how the various spans of bridge open and close together.
Thanks to its prestige as a unique example of bridge engineering, there was a higher-than-usual amount of technical literature available on the Hörn bridge's design.
In turn, this gave me some much-needed insight into the physics of the bridge and how it unfolds.
From there, it was all about coding and execution in Mathematica.

<div class="feature-image">
    <a href="{{ site.url }}/{{ site.assets.features }}/{{ page.specifics.images }}/01_deploy.jpg">
        <img src="{{ site.url }}/{{ site.assets.features }}/{{ page.specifics.images }}/01_deploy.jpg" width="800" style="margin:4px 4px 4px 4px">
    </a>
</div>

The Mathematica project went through three different stages of development:

* **Initial problem setup & physical characterization**
* **Finding the correct constraints on the bridge's operation**
* **Modeling the impact condition and attempting some form of control over the bridge's action**

The final form of the project is an animation of the bridge unfolding under the effects of gravity and experiencing an impact on the far side pylon.
I implemented some basic controls, mostly so that the framework would exist should I get the chance to return later and implement some form of active control sufficient to safely deploy the bridge.
At the time the project was submitted, it could only delay (or accelerate) the opening of the bridge under gravity, which then results in a pretty forceful collision on the furthest pylon.

<div class="feature-image">
    <a href="{{ site.url }}/{{ site.assets.features }}/{{ page.specifics.images }}/02_results.jpg">
        <img src="{{ site.url }}/{{ site.assets.features }}/{{ page.specifics.images }}/02_results.jpg" width="940" style="margin:4px 4px 4px 4px">
    </a>
</div>

This project was an incredible amount of fun to work on.
For the purposes of getting some meaningful data out of the simulation, I made sure to take a reading of the velocity experienced at the end of the final span at the moment of impact.
After having taken that reading, it's easy to conclude that the simulated bridge hits far too hard to be considered "safe" to just let gravity act on the bridge.
I haven't come back to this project, mostly because I no longer have access to a Mathematica license, but the analytical solution for the bridge could be pretty easily ported over to another simulation environment and would serve as a great reference project.
For those who would be interested in referencing on the dynamic simulation already in place, or those curious to see an example of how I structured the evaluation and animation components, the source code for the project is available over on **[GitHub](https://github.com/spieswl/horn-bridge-dynamics_sim)**!

**BONUS:** Here's a GIF of the bridge deploying out of an initially-folded configuration.

<div class="feature-image">
    <a href="{{ site.url }}/{{ site.assets.features }}/{{ page.specifics.images }}/03_animation.gif">
        <img src="{{ site.url }}/{{ site.assets.features }}/{{ page.specifics.images }}/03_animation.gif" width="480">
    </a>
</div>
