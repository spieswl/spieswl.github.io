---
layout:             project
title:              "Building a Go-Kart Robot from Scratch"
date:               "2018-06-14"

description:        "Building a robot from nothing crosses a variety of disciplines; I walk through the process of building a simple line-following robot from scratch."
keywords:           robotics, design, mechanical engineering, electrical engineering, programming, fabrication, advanced manufacturing, laser cutting, 3D printing, onshape
tags:               [Design, Robotics]

specifics:
    featured:       true
    images:         "cotton-candy-kart"

published:          true
---

During my time in Northwestern's **MSR** program, I took a series of Mechatronics courses that covered several different aspects of robot design. The capstone of the series was a design project to build a shoebox-sized robotic go-kart from scratch and program it to drive autonomously around a custom race track. My go-kart (_affectionately nicknamed Cotton Candy after its color scheme_) was designed and built over several weeks worth of time in early 2018. Every aspect of the go-kart creation, including mechanical design, electrical controls (_with one exception that was provided by the class administrator_), the software running on the microcontroller and an attached Android phone, and vehicle assembly, was ultimately my responsibility during the lead-up to the final demonstration. It was a great way to accrue a bunch of cross-disciplinary design experience, and the 'race' at the end was exciting for the chance to see all of the other designs and talk shop about the process.

[Nick Marchuk](http://www.mccormick.northwestern.edu/research-faculty/directory/affiliated/marchuk-nicholas.html), administrator of the NU Mechatronics classes, started our series by kicking us off with workshops that focused on a range of topics in robotic systems design, such as designing controls electronics for motor control, digital signal processing for sensor noise compensation, sensor fusion, mechanical CAD design, and other topics. He also provided us with a small motor control daughter board, designed to work with our self-designed microcontroller PCBs. Lastly, we each created an **[OnShape](https://www.onshape.com/)** profile, as we would be performing our mechanical design through their online CAD system.

With the stage set, the rest of this project write-up just goes through all the discipline-specific areas and talks about my experiences with each. First, look over at GitHub for [the project repository](https://github.com/spieswl/robotic-kart-v1) which holds the mechanical designs, electrical schematics, and the microcontroller and Android application source code. _**[MPLAB IDE](http://www.microchip.com/mplab/mplab-x-ide)**_ and _**[Android Studio](https://developer.android.com/studio/)**_ were used for emdedded device programming, and _**[Autodesk EAGLE](https://www.autodesk.com/products/eagle/overview)**_ was used for the electrical schematics and PCB layout.

<div class="project-image">
    <a href="{{ site.url }}/{{ site.assets.projects }}/{{ page.specifics.images }}/thumb.png">
        <img src="{{ site.url }}/{{ site.assets.projects }}/{{ page.specifics.images }}/thumb.png" width="400" style="padding:0px 0px 0px 0px;">
    </a>
    <a href="{{ site.url }}/{{ site.assets.projects }}/{{ page.specifics.images }}/01_finished_front.png">
        <img src="{{ site.url }}/{{ site.assets.projects }}/{{ page.specifics.images }}/01_finished_front.png" width="533" style="padding:0px 0px 0px 0px;">
    </a>
</div>

<div class="project-image">
    <a href="{{ site.url }}/{{ site.assets.projects }}/{{ page.specifics.images }}/02_finished_side.png">
        <img src="{{ site.url }}/{{ site.assets.projects }}/{{ page.specifics.images }}/02_finished_side.png" width="533" style="padding:0px 0px 0px 0px;">
    </a>
    <a href="{{ site.url }}/{{ site.assets.projects }}/{{ page.specifics.images }}/03_finished_top.png">
        <img src="{{ site.url }}/{{ site.assets.projects }}/{{ page.specifics.images }}/03_finished_top.png" width="400" style="padding:0px 0px 0px 0px;">
    </a>
</div>

The vehicle itself is a differential-drive system, with the wheels driven by two Chihai **[CHR-16G-050-ABHL](https://www.aliexpress.com/item/Chihai-Motor-CHR-16G-050-ABHL-DC-6V-12V-7PPR-Encoder-Motor-Reducer-Carbon-brush-Gear/32833392055.html)** 12V, 3.3W DC motors and controlled with a **[PIC32MX250F128B](https://www.digikey.com/product-detail/en/microchip-technology/PIC32MX250F128B-I-SP/PIC32MX250F128B-I-SP-ND/3046657)** microcontroller. The Android phone on top (_on my vehicle, this phone is a **[OnePlus One](https://en.wikipedia.org/wiki/OnePlus_One#Specifications)** running Android Marshmallow_) is needed for its rear-facing camera. The nominal design uses the phone's rear-facing camera to perform line detection via a custom Android application that was sideloaded onto the phone. The rear-most caster, 12V lithium-ion battery, wheel collets/caps, and miscellaneous wiring and fasteners were all hand-picked, although in the battery's case we were given limited options to choose from. All of the mechanical chassis pieces were designed by myself and either 3D-printed (with PLA) or laser cut (from cast acrylic sheets).

<hr>

#### Mechanical Design

The mechanical design of the vehicle was where I got most creative. All mechanical design was done in OnShape, and you can click on the preview images below to inspect the final detail drawings (which are included in the GitHub repo).

<div class="project-image">
    <a href="https://github.com/spieswl/robotic-kart-v1/blob/master/mech/CCK-WH01-01_DD.pdf">
        <img src="{{ site.url }}/{{ site.assets.projects }}/{{ page.specifics.images }}/m1_wheel_design.png" width="500" style="padding:0px 6px 0px 6px;">
    </a>
    <a href="https://github.com/spieswl/robotic-kart-v1/blob/master/mech/CCK-CHAS01-01_DD.pdf">
        <img src="{{ site.url }}/{{ site.assets.projects }}/{{ page.specifics.images }}/m2_chassis_design.png" width="500" style="padding:0px 6px 0px 6px;">
    </a>
</div>

The wheels are a vibrant shade of pink as our 3D printer in the MSR lab only had pink filament available. As it turns out, the use of color gave the finished product a nice look (though I wish I had picked out blue wheel hubs, instead of the red hubs). The twin wheel channels for O-rings was a particularly savvy idea, as it allowed for more grip and ground contact when compared to some of the thinner wheel designs that only had room for a single O-ring. The trade-off was a longer 3D print time, but the improved performance on inclines was well worth the extra time spent on the print bed.

The chassis (both the mounting surface and the front phone support bracket) was cut from a single blue sheet of acrylic. Thanks to the [Ford Engineering Design Center](http://design.northwestern.edu/about/design-facilities/prototyping-lab.html)'s laser cutters, the mounting holes and other cutout details were a breeze to handle. Nearly everything was secured with assorted bolts, washers, and nuts, although the motor power/encoder cables were secured with tie-wraps to keep those from moving around when the kart was driving.

The phone support bracket at the front of the vehicle was specifically designed to elevate the top portion of the phone. With a slight upward angle to the phone, the rear-facing camera is able to look slightly ahead of the drive axis.

The battery and motors are clamped to the chassis with custom-fit, 3D-printed pieces. The control electronics are on stand-offs to allow some cable routing underneath, and also to provide a secure mounting point to the rest of the body. As a result of all this, excluding the phone, the vehicle has no loose parts.

<hr>

#### Electrical Design

The electrical design was oriented around several design constraints, most of which involved the need for peripheral motor control and a USB OTG interface to the Android phone. As mentioned earlier, core processing was handled with a 32-bit **PIC32MX250F128B** microcontroller. Most components were sourced from either **DigiKey** or **Sparkfun**, although there were a ton of components laying around the lab that our classes were also permitted to use. The left two images show the final, populated, soldered control board, before I mounted it to the chassis.

<div class="project-image">
    <a href="{{ site.url }}/{{ site.assets.projects }}/{{ page.specifics.images }}/05_elecPCB_top.png">
        <img src="{{ site.url }}/{{ site.assets.projects }}/{{ page.specifics.images }}/05_elecPCB_top.png" width="350" style="padding:0px 0px 0px 0px;">
    </a>
    <a href="{{ site.url }}/{{ site.assets.projects }}/{{ page.specifics.images }}/06_elecPCB_bottom.png">
        <img src="{{ site.url }}/{{ site.assets.projects }}/{{ page.specifics.images }}/06_elecPCB_bottom.png" width="350" style="padding:0px 0px 0px 0px;">
    </a>
    <a href="{{ site.url }}/{{ site.assets.projects }}/{{ page.specifics.images }}/07_motorPCB_top.png">
        <img src="{{ site.url }}/{{ site.assets.projects }}/{{ page.specifics.images }}/07_motorPCB_top.png" width="350" style="padding:0px 0px 0px 0px;">
    </a>
</div>

The supplied daughter board, which had a ready-to-go design for PWM motor control, can be seen on the far right. Our responsibilities were simply to populate the board and plug in the peripherals.

Spacing for the 8-pin header interface provided the only position constraint for the PCB design, though I elected to use surface-mount components in order to cut down the amount of vias needed to connect everything. The system design includes the microcontroller, two status LEDs (_one power, one user-configurable_), the power supply, two pushbuttons (_one master reset, one user-configurable_), the programming port, the USB Mini port, and the headers for the motor controller board. Schematic and layout were both done with Eagle, and are shown below.

<div class="project-image">
    <a href="https://github.com/spieswl/robotic-kart-v1/blob/master/elec/controller_schematic.pdf">
        <img src="{{ site.url }}/{{ site.assets.projects }}/{{ page.specifics.images }}/e1_controller_schematic.png" width="637" style="padding:0px 0px 0px 0px;">
    </a>
    <a href="https://github.com/spieswl/robotic-kart-v1/blob/master/elec/controller_layout.pdf">
        <img src="{{ site.url }}/{{ site.assets.projects }}/{{ page.specifics.images }}/e2_controller_layout.png" width="450" style="padding:0px 0px 0px 0px;">
    </a>
</div>

All members of the ME433 class used a third-party PCB manufacturer, [PCBWay](https://www.pcbway.com/), to do the board manufacturing. I did the all of the soldering and board testing myself, so I was pretty happy when everything worked like a charm. I also really like the nice, clean look that my PCB ended up with, and the post holes in the corner were exactly what was needed to secure the standoffs, resulting in a sturdy connection to the rest of the chassis.

<hr>

#### Software Design

The software design was split into two distinct parts:

1. **The main microcontroller program,** and
2. **The line-finding Android application.**

<div class="project-image">
    <a href="{{ site.url }}/{{ site.assets.projects }}/{{ page.specifics.images }}/08_techcuptrack.png">
        <img src="{{ site.url }}/{{ site.assets.projects }}/{{ page.specifics.images }}/08_techcuptrack.png" width="400" style="margin:24px; float:right">
    </a>
</div>

The high-level structure of the software is as follows: The microcontroller code, written in `C`, initializes and sets up an internal state machine. It then immediately attempts to open a socket to the Android phone via USB OTG. The microcontroller uses data from the phone application, written in `Java`, to determine how to drive the motors via a **PI control** loop. The system continually runs through the **[app<->PI control]** sequence until powered down.

The microcontroller uses a single PI controller, looping at a modest (about 50 Hertz) frequency to output two individual PWM signals (_one for each motor_). The PWM signals are clamped to fall in a range that corresponds to 0-100% motor power. Depending on where the microcontroller "perceives" the line to be, it will raise or lower the left or right motor power appropriately, while attempting to maintain some amount of forward velocity. The PWM frequency itself is set to about 20 kilohertz.

The microcontroller "perceives" the line position by being told where the line is in the camera's field of view. Thanks to `Harmony`, a set of Microchip-provided libraries, the microcontroller can receive data via **USB OTG** while a phone application is running. The Android application intercepts the camera stream and does color segmentation to identify a specific, colored line on the track-in this case, I am picking out the red portion of the line. The application passes along the line position by passing an integer value that represents the line position in pixel coordinates on the rear-facing camera image plane. Since I know how wide the camera image is, I can set center-image values to correspond to equal power applied in the forward direction for both motors. Everything outside of the center then triggers a scaled  change in applied motor power.

There are plenty of other clever things done in the code, such as exposing controls for color saturation and intensity thresholds that can be tuned on the phone's display, to more easily work around ambient lighting changes.

You can see an example of the course at right. **NOTE** that there is an inclined ramp cross-over, which isn't shown on this image file, that connects the vertical straightaway and closes the loop on the track.

As for the competition, my cart traversed the course in just over 2 minutes, which put it into the top quarter of the time trials. The incline cross-over in the middle gave many of the other teams all kinds of trouble, since most of the robots could not generate enough traction to get up the slope, so most of the results ended up being marked down as "Did Not Finish". I cannot complain about my robot getting relatively good results, yet I am happy with how this robot evolved even if I just look at it from a design perspective. I also think it would be a neat extension to come back and iterate on this robot's design, if I can find a more practical use-case for such a robot in the future.
