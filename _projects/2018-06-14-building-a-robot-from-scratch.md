---
layout:             project
title:              "Building a Robotic Kart from Scratch"
date:               "2018-06-14"

description:        "Building a robot from nothing involves a lot of work across a variety of disciplines; we go through the full process of building a line-following robot from scratch."
keywords:           embedded systems, C, android, mechanical engineering, electrical engineering, schematic entry, fabrication, advanced manufacturing, laser cutting, 3D printing, onshape
tags:               [C, Android, Robotics, Embedded Systems, Mechanical, Electrical, 3D Printing, Laser Cutting, OnShape]

folders:
  images:           "cotton-kandy-kart"                          # This path is project-dependent; don't forget to change it!

published:          true
---

The ultimate aim of a series of Mechatronics courses I took for the MSR program was a project to build a robotic kart from scratch and let it drive autonomously around a race track as part of a design competition. My kart, nicknamed Cotton Kandy (_check out the vehicle color scheme_), was designed and built over the course of several weeks in the Spring of 2018. Every aspect of the kart, including the mechanical design, the electrical controls (with one exception that provided by the class administrator), the software running on the microcontroller and on an Android cell phone, and the assembly of the vehicle was all handled by myself during the lead-up to the competition. As a result, I accrued significant cross-disciplinary design experience...and I had a fantastic time while doing so.

[Nick Marchuk](http://www.mccormick.northwestern.edu/research-faculty/directory/affiliated/marchuk-nicholas.html), administrator of the Mechatronics classes, started our project span by kicking us off with workshops that focused on a plethora of topics in embedded systems design, such as designing electronics for PCB layout, digital signal processing for sensor noise compensation, sensor fusion, mechanical CAD design, and other topics. He also provided us with a small motor control daughter board, designed to work with our self-designed microcontroller PCBs. Lastly, we were required to create an **[OnShape](https://www.onshape.com/)** profile, as we would be performing our mechanical design through their online CAD system.

With the stage set, the rest of this project write-up is going to walk you through all of the discipline areas and talk about my experiences designing each aspect of the kart. First, **[look over at GitHub for the roll-up repository](https://github.com/spieswl/cotton-kandy-kart)** holding mechanical designs (_pardon the PDFs_), electrical schematics, and the necessary software. Note that _**[MPLAB IDE](http://www.microchip.com/mplab/mplab-x-ide)**_ and _**[Android Studio](https://developer.android.com/studio/)**_ were used to perform the programming, and _**[Autodesk EAGLE](https://www.autodesk.com/products/eagle/overview)**_ was used for the electrical schematics and PCB layout. 

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

The vehicle itself is a differential-drive system, with the wheels driven by two Chihai **[CHR-16G-050-ABHL](https://www.aliexpress.com/item/Chihai-Motor-CHR-16G-050-ABHL-DC-6V-12V-7PPR-Encoder-Motor-Reducer-Carbon-brush-Gear/32833392055.html)** 12V, 3.3W DC motors and controlled with a **[PIC32MX250F128B](https://www.digikey.com/product-detail/en/microchip-technology/PIC32MX250F128B-I-SP/PIC32MX250F128B-I-SP-ND/3046657)** microcontroller. The Android phone on top (on my vehicle, this phone is a **[OnePlus One](https://en.wikipedia.org/wiki/OnePlus_One#Specifications)** running Android Marshmallow) is needed for its rear-facing camera. My design uses the phone's camera subsystem to perform line acquisition via a custom application written specifically for this project. The rear caster, 12V lithium-ion battery, wheel collets/caps, and miscellaneous wiring and fasteners were all hand-picked, though, in the case of the battery, we were given limited options to choose from. All of the mechanical chassis pieces were designed by myself and either 3D-printed (with PLA) or laser cut (from cast acrylic).

<hr/>

#### Mechanical Design

The mechanical design of the vehicle was where I was able to get the most creative. All mechanical design was done in OnShape, and you can click on the preview images below to inspect the final detail drawings included in the GitHub repository.

<div class="project-image">
    <a href="https://github.com/spieswl/cotton-kandy-kart/blob/master/mech/CKK-WH01-01_DD_v6.pdf">
        <img src="{{ site.url }}/{{ site.project_assets }}/{{ page.folders.images }}/m1_wheel_design.png" style="width:500px; padding:0px 6px 0px 6px;">
    </a>
    <a href="https://github.com/spieswl/cotton-kandy-kart/blob/master/mech/CKK-CHAS01-01_DD_v2.pdf">
        <img src="{{ site.url }}/{{ site.project_assets }}/{{ page.folders.images }}/m2_chassis_design.png" style="width:500px; padding:0px 6px 0px 6px;">
    </a>
</div>

The wheels are a vibrant shade of pink as our 3D printer in the MSR lab ran out of red PLA before I could print any of the wheels out. Even so, I like the use of color throughout (though I wish I had picked out blue wheel hubs, instead of the red hubs I started out with). The twin channels for O-Rings was a particularly savvy idea, as it allowed for more grip and ground contact when compared to a thinner wheel that might only have room for a single channel. The trade-off was a longer print time, but the improved performance was well worth the cost.

The chassis (both the mounting surface and the front phone support bracket) was cut from a single blue sheet of acrylic. Thanks to the [Ford Engineering Design Center](http://design.northwestern.edu/about/design-facilities/prototyping-lab.html)'s laser cutters, making the mounting holes and other cutout details a breeze to handle. Nearly everything was secured with bolts, washers, and nuts, although the motor power/encoder cables were secured with tie-wraps to keep those from moving around during operation.

The phone support bracket at the front of the vehicle is sized to elevate the rear-facing camera on the phone just enough to look slightly ahead of the drive axis.

The battery and motors are clamped to the chassis with custom-fit, 3D-printed pieces. The control electronics are on stand-offs to allow some cable routing underneath, and also to provide a secure mounting point to the rest of the body. As a result of all this, excluding the phone, the vehicle has no loose parts.

<hr/>

#### Electrical Design

The electrical design was oriented around several design constraints, most of which involved the need for peripheral motor control and a USB interface to the phone. As mentioned earlier, core processing was handled with a 32-bit **PIC32MX250F128B** microcontroller. The components were sourced from either **DigiKey** or **Sparkfun**, although there were a ton of components laying around the lab that our class was also permitted to use. The left two images show the final, populated, soldered control board, right before I mounted it to the chassis.

<div class="project-image">
    <a href="{{ site.url }}/{{ site.project_assets }}/{{ page.folders.images }}/05_elecPCB_top.png">
        <img src="{{ site.url }}/{{ site.project_assets }}/{{ page.folders.images }}/05_elecPCB_top.png" style="width:350px; padding:0px 0px 0px 0px;">
    </a>
    <a href="{{ site.url }}/{{ site.project_assets }}/{{ page.folders.images }}/06_elecPCB_bottom.png">
        <img src="{{ site.url }}/{{ site.project_assets }}/{{ page.folders.images }}/06_elecPCB_bottom.png" style="width:350px; padding:0px 0px 0px 0px;">
    </a>
    <a href="{{ site.url }}/{{ site.project_assets }}/{{ page.folders.images }}/07_motorPCB_top.png">
        <img src="{{ site.url }}/{{ site.project_assets }}/{{ page.folders.images }}/07_motorPCB_top.png" style="width:350px; padding:0px 0px 0px 0px;">
    </a>
</div>

The supplied daughter board, which had a ready-to-go design for PWM motor control, can be seen on the far right. Our responsibilities were simply to populate the board and plug in the peripherals.

Spacing for the 8-pin header interface provided the only position constraint for the PCB design, though I elected to use surface-mount components in order to cut down the amount of vias needed to route the system. The system design includes the microcontroller, two status LEDs (_one power, one user-configurable_), the power supply, two pushbuttons (_one master reset, one user-configurable_), the programming port, the USB Mini port, and the headers for the motor controller board. Schematic and layout were both done with Eagle, and are linked below.

<div class="project-image">
    <a href="https://github.com/spieswl/cotton-kandy-kart/blob/master/elec/controller_schematic.pdf">
        <img src="{{ site.url }}/{{ site.project_assets }}/{{ page.folders.images }}/e1_controller_schematic.png" style="height:450px; padding:0px 0px 0px 0px;">
    </a>
    <a href="https://github.com/spieswl/cotton-kandy-kart/blob/master/elec/controller_layout.pdf">
        <img src="{{ site.url }}/{{ site.project_assets }}/{{ page.folders.images }}/e2_controller_layout.png" style="width:450px; padding:0px 0px 0px 0px;">
    </a>
</div>

All members of the ME433 class used a third-party PCB manufacturer, [PCBWay](https://www.pcbway.com/), to do the board manufacturing. I did the all of the soldering and board testing, however, so I was pretty happy when everything worked like a charm. I also really like the nice, clean look that my PCB ended up with, and the post holes in the corner were exactly what was needed to secure the standoffs, resulting in a sturdy mechanical link to the rest of the system.

<hr/>

#### Software Design

The software design was split into two distinct parts:

1. **The main microcontroller program,** and
2. **The line-finding Android application.**

<div class="project-image">
    <a href="{{ site.url }}/{{ site.project_assets }}/{{ page.folders.images }}/08_techcuptrack.png">
        <img src="{{ site.url }}/{{ site.project_assets }}/{{ page.folders.images }}/08_techcuptrack.png" style="width:400px; margin:24px; float:right">
    </a>
</div>

The high-level structure of the code is as follows: The microcontroller code, written in `C`, initializes and sets up an internal state machine. It then immediately attempts to open a socket to the Android phone via USB OTG. The microcontroller uses data from the phone application, written in `Java`, to determine how to drive the motors via a **PI control** loop. The system continually runs through the **[app<->PI control]** sequence until powered down.

The microcontroller uses a single PI controller, at a modest (about 50 Hertz) frequency to output two individual PWM signals. The PWM signals are clamped to fall in a range that corresponds to 0-100% motor power. Depending on where the microcontroller "perceives" the line to be, it will raise or lower the left or right motor power appropriately, while attempting to maintain some amount of forward velocity. The PWM frequency itself is set to about 20 kiloHertz.

The microcontroller "perceives" the line position by being told where the line is in the camera's field of view. Thanks to `Harmony`, a set of Microchip-provided libraries, the microcontroller can receive data via **USB OTG** while the phone application is running. The Android application intercepts the camera stream and does color segmentation to identify a specific, colored line on the track-in this case, I am picking out the red portion of the line. The application passes along the line position by passing an integer value that represents the line position in pixel coordinates on the rear-facing image plane.  Since I know how wide the camera image is, I can set center-image values to correspond to equal power applied in the forward direction for both motors. Everything outside of the center then triggers a change in applied motor power.

There are plenty of other clever things done in the code, such as exposing controls for color saturation and intensity thresholds that can be tuned on the phone's display as the ambient lighting changes. If you wish to check out the code, load it up in the respective IDE and check it out!

You can see an example of the course at right. **NOTE** that there is a ramp cross-over, which isn't shown on this image file, that connects the vertical straightaway and closes the loop on the track.

<br>
<br>


...and that is essentially it! The kart is still sitting above my desk there in the Northwestern labs, and I frequently get questions and comments about it. The battery definitely needs a charge cycle, but otherwise this kart could easily serve as a platform for other robotic projects, if I were to further iterate on the system design. If you wish to pass along questions or comments of your own, please check out the **[About]({{site.url}}/about)** page and drop me a line!
