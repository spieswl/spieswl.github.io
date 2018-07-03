---
layout:             project
title:              "Building a Robotic Kart from Scratch"
date:               "2018-06-14"

description:        "Building a robot from nothing involves a lot of work across a variety of disciplines; we go through the full process of building a line-following robot from scratch."
keywords:           embedded systems, C, android, mechanical engineering, electrical engineering, schematic entry, fabrication, advanced manufacturing, laser cutting, 3d printing, onshape
tags:               [C, Android, Robotics, Embedded Systems, Mechanical, Electrical, 3D Printing, Laser Cutting, OnShape]

folders:
  images:           "cotton-kandy-kart"                          # This path is project-dependent; don't forget to change it!

published:          false
---

The culmination of a series of Mechatronics classes I took for the MSR program was a project to build a robotic kart from scratch and have it autonomously drive around a race course as part of a design competition. My kart, nicknamed Cotton Kandy (_the vehicle color scheme lent itself to that name_), was designed and built over the course of several weeks in the Spring of 2018. Every aspect of the kart, including the mechanical design, the electrical controls (with one exception provided by the class administrator), the software running on the microcontroller and on an Android cell phone, and the assembly of the vehicle was all handled by myself during the lead-up to the competition. As a result, I accrued significant cross-disciplinary design experience...and I had an incredible time in doing so.

[Nick Marchuk](http://www.mccormick.northwestern.edu/research-faculty/directory/affiliated/marchuk-nicholas.html), administrator of the Mechatronics classes, started our project span by kicking us off with workshops that focused on a plethora of topics in embedded systems design, such as designing electronics for PCB layout, digital signal processing for sensor noise compensation, sensor fusion, mechanical CAD design, and other topics. He also provided us with a small motor control daughter board, designed to work with our self-designed microcontroller PCBs. Lastly, we were required to create an **[OnShape](https://www.onshape.com/)** profile, as we would be performing our mechanical design through their online CAD system.

With the stage set, the rest of this project write-up is going to walk you through all of the discipline areas and talk about my experiences designing each aspect of the kart. First, a look at the finished product:

<div class="project-image">
    <a href="{{ site.url }}/{{ site.project_assets }}/{{ page.folders.images }}/thumb.png">
        <img src="{{ site.url }}/{{ site.project_assets }}/{{ page.folders.images }}/thumb.png" style="width:400px; padding:0px 0px 0px 0px;">
    </a>
    <a href="{{ site.url }}/{{ site.project_assets }}/{{ page.folders.images }}/01_finished_front.png">
        <img src="{{ site.url }}/{{ site.project_assets }}/{{ page.folders.images }}/01_finished_front.png" style="width:533px; padding:0px 0px 0px 0px;">
    </a>
</div>

<div class="project-image">
    <a href="{{ site.url }}/{{ site.project_assets }}/{{ page.folders.images }}/02_finished_side.png">
        <img src="{{ site.url }}/{{ site.project_assets }}/{{ page.folders.images }}/02_finished_side.png" style="width:533px; padding:0px 0px 0px 0px;">
    </a>
    <a href="{{ site.url }}/{{ site.project_assets }}/{{ page.folders.images }}/03_finished_top.png">
        <img src="{{ site.url }}/{{ site.project_assets }}/{{ page.folders.images }}/03_finished_top.png" style="width:400px; padding:0px 0px 0px 0px;">
    </a>
</div>

The kart itself is a differential-drive system, with the wheels driven by two Chihai **[CHR-16G-050-ABHL](https://www.aliexpress.com/item/Chihai-Motor-CHR-16G-050-ABHL-DC-6V-12V-7PPR-Encoder-Motor-Reducer-Carbon-brush-Gear/32833392055.html)** 12V, 3.3W DC motors and controlled with a **[PIC32MX250F128B](https://www.digikey.com/product-detail/en/microchip-technology/PIC32MX250F128B-I-SP/PIC32MX250F128B-I-SP-ND/3046657)** microcontroller. The Android phone on top (on my kart, this phone is a **[OnePlus One](https://en.wikipedia.org/wiki/OnePlus_One#Specifications)** running Android Marshmallow) is needed for its forward-facing camera. My design uses the phone's camera subsystem to perform line acquisition via a custom application written specifically for this project. The rear caster, 12V lithium-ion battery, wheel collets / caps, and miscellaneous wiring and fasteners were all hand-chosen, though, in the case of the battery, we were given limited options to choose between. All of the mechanical chassis pieces were designed by myself and either 3D printed (with PLA) or laser cut (from cast acrylic).

#### Mechanical Design

The mechanical design of the kart was where I was able to get the most creative.

The wheels ended up being pink because our 3D printer in the MSR lab ran out of red PLA before I could print any of the wheels out. Even so, I like the use of color throughout (though I wish I had picked out blue wheel hubs, instead of the red hubs I started out with).

#### Electrical Design

#### Software Design

#### Assembly

#### Results

<!--
<TODO : ## Lessons Learned?>
<TODO : ## Future Work>
-->
