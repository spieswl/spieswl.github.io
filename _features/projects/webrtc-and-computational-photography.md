---
layout:             feature
title:              "webrtc-perception : Using WebRTC for Computational Photography"
date:               "2018-11-14"

description:        "Devices capable of providing images for computational applications are abundant but few web frameworks are freely available for academic, artistic, or scientific use. This project contains examples of using WebRTC for surface reconstruction, surface measurement, and near real-time modeling with consumer-grade mobile devices."
keywords:           webrtc, javascript, computer vision, computational photography, image processing, photometric stereo, phase measured deflectometry
tags:               [WebRTC, Deflectometry, Python, Computer Vision]

specifics:
    career:         false
    project:        true
    images:         "webrtc-perception"

published:          true
deadhead:           false
---

During my time spent in Northwestern University's **[Computational Photography Lab](http://compphotolab.northwestern.edu/)**, I divided my attention between a handheld 3D scanner project and another project oriented around **[WebRTC](https://webrtc.org/)**.
WebRTC (_RTC stands for Real-Time Communications_) is a suite of APIs that enables the capture and transfer of video and audio content entirely through a web browser.
Several applications and products already leverage WebRTC for video conferencing, gaming, media sharing, and other social applications, so it has benefited from steady growth and support since its introduction at the [2013 Google I/O developers conference](https://www.youtube.com/watch?v=p2HzZkd2A40).
Some developers and researchers have also used WebRTC to facilitate IoT applications, serve as the framework for hobbyist projects, and have integrated it into cutting-edge computer science and robotics research.

I was asked to look into WebRTC and its APIs in mid-2018 to determine if our lab could use this technology as the basis for a new scientific data collection system.
My objective was to develop a pattern-transmission and image-capture framework that could be used for multiple upcoming research projects.
Furthermore, my system needed to work without requiring my colleagues to possess special hardware or be familiar with the nuances of browsers and web frameworks.
Our lab also looked at this project as a chance to create a system that could be used by individuals in broader science contexts, namely art curators and conservators, for historical or scientific purposes.
The most imposing limitation was that the end system could not require users to download a separate application, and instead ONLY use what would be available in modern web browsers.
This did put a significant constraint on the potential capabilities, but the tradeoff was that it ensured the broadest possible audience.

The design of **webrtc-perception** includes a capture website, a dedicated server for processing image data, and a results display website.
Since `WebRTC` is used for capture and transport, users have to rely on other resources to complete their application, such as a dedicated server to handle image and data processing tasks and return useful results.
This also confers some advantages, as operators can improve the processing code on the fly, change camera controls and presentation details on the respective websites, and fix issues without users needing to download or install any updates.
This project also leans on a Python library, named `aiortc`, to implement asynchronous interaction with connecting clients via `WebRTC` and perform useful computation on images and other data gathered through its use.
Jeremy Lainé has put together a very useful package and I highly recommend [giving it a closer look](https://github.com/aiortc/aiortc).

Finally, there are some details below the **webrtc-perception** metapackage description that talks about some specific applications for this technology, both of which have unique implications for scientific study of artistic works.

<hr>

#### webrtc-perception

The project "metapackage" is named **webrtc-perception** and is hosted [over on GitHub](https://github.com/spieswl/webrtc-perception).
Examples of application-specific code is contained within the "content" folder, while the metapackage itself serves as the issue tracker and documentation holder for all contained content.
At present, two applications are featured in the metapackage: `rtc-shapeshifter` and `rtc-deflectometry`.
Each application is connected to specific active research projects in the **Computational Photography Lab**.

A barebones illustration of the **webrtc-perception** framework is shown in the following figure.
This gives you an idea of what an end-to-end system _could_ look like, but without the _rtc-shapeshifter_- or _rtc-deflectometry_-specific details.

<div class="feature-image">
    <a href="https://raw.githubusercontent.com/spieswl/webrtc-perception/master/docs/webrtc-perception_block_diagram.png">
        <img src="https://raw.githubusercontent.com/spieswl/webrtc-perception/master/docs/webrtc-perception_block_diagram.png" width="800">
    </a>
</div>

**webrtc-perception** uses the WebRTC framework to establish a connection between a server and a client device in a seamless manner.
`getUserMedia()` and other _MediaStream_ components simplify connecting to a client device.
The client device, thanks to other `MediaStream` features, also permits the server to detect and choose which photography settings are important for that particular camera track (such as **_exposure time_**, **_ISO_**, **_white balance_**, **_focus distance_**, rear **_torch_** status, etc).
The client signals to the server when it is ready to begin data capture, and the server responds with a signal to start "measuring" with the device.
The server handles gathering data from the client and performs application-specific computation on all the gathered data.
The server does all this through the use of Python and `aiortc` to connect with a client via WebRTC without needing to use a web browser itself.
The Python code converts the results of the computation into a format which can be transmitted to another, separate website designed to display (_and make available, if necessary_) the results.
The featured implementations attempt to do this as close to real-time as possible, so that the user in control of the measurement client can evaluate the measurement process in a sort of feedback loop.

The next sections outline the goals of `rtc-shapeshifter` and `rtc-deflectometry` and how my colleagues used **webrtc-perception** to achieve those goals.

##### > rtc-shapeshifter

`rtc-shapeshifter` is a WebRTC-based tool that expands upon a concept originally presented by [Chia-Kai Yeh](https://github.com/kaiyeh0913) called _[Shape by Shifting](https://ieeexplore.ieee.org/abstract/document/8109194)_.
His work originally used DSLR cameras to get preliminary results and he switched to using an iPhone (_with some special hardware_) in its final form, which made it an interesting candidate for extension through **webrtc-perception**.
While I will not go into deep technical detail on his work, I included some slides from a presentation we held for one of the university's scientific interest groups on October 19th, 2018:

<div class="feature-image">
    <a href="{{ site.url }}/{{ site.assets.features }}/{{ page.specifics.images }}/01_sfs_slide_1.png">
        <img src="{{ site.url }}/{{ site.assets.features }}/{{ page.specifics.images }}/01_sfs_slide_1.png" width="600" style="padding:0px 0px 0px 0px;">
    </a>
    <a href="{{ site.url }}/{{ site.assets.features }}/{{ page.specifics.images }}/02_sfs_slide_2.png">
        <img src="{{ site.url }}/{{ site.assets.features }}/{{ page.specifics.images }}/02_sfs_slide_2.png" width="600" style="padding:0px 0px 0px 0px;">
    </a>
</div>

In short, Kai has been using the **webrtc-perception** framework to make it easier for him to recover surface normal maps with an off-the-shelf **NVIDIA SHIELD K1** tablet though the use of photometric stereo measurement.
He can control various photography settings remotely, trigger image capture from the rear-facing camera (_WITH the LED light enabled_), clip on his polarizer, and automate the image processing and results generation.
Thanks to the design of **webrtc-perception**, he can see his results _while_ he is capturing data.
This system has made it far easier to perform surface measurements of painted works of art for the purposes of preservation and restoration.
Our work was presented at 2019's **AAAS** conference and highlighted by AAAS on _Science_ magazine's website, as well as featured on Northwestern University's Engineering News reel.

* [https://www.sciencemag.org/news/2019/02/new-app-reveals-hidden-landscapes-within-georgia-o-keeffe-s-paintings](https://www.sciencemag.org/news/2019/02/new-app-reveals-hidden-landscapes-within-georgia-o-keeffe-s-paintings)
* [https://www.mccormick.northwestern.edu/news/articles/2019/02/diagnosing-art-acne-in-georgia-okeeffe-paintings.html](https://www.mccormick.northwestern.edu/news/articles/2019/02/diagnosing-art-acne-in-georgia-okeeffe-paintings.html)

##### > rtc-deflectometry

`rtc-deflectometry` is a WebRTC-based tool that implements **_Phase Measuring Deflectometry (PMD)_** in order to optically measure surfaces that exhibit specular reflection.
PMD, for the unfamiliar, can be described as projecting light in varying structured patterns and using a camera element to perceive how a surface affects the reflection of the pattern.
In particular, Dr. Florian Willomitzer, the leading CPL post-doctoral researcher, was interested in scanning particular glass tiles that were present in the lab.
These glass tiles are part of a sample set from the **[Kokomo Opalescent Glass Works](https://en.wikipedia.org/wiki/Kokomo_Opalescent_Glass_Works)** in Indiana, famous for having supplied glass media to Louis Comfort Tiffany (_the visionary mind behind "Tiffany glass"_).
These sample tiles have a particular surface shape that can be traced to Kokomo's specific [roller table process](https://en.wikipedia.org/wiki/Architectural_glass#Rolled_plate_(figured)_glass).
Pieces commissioned by Tiffany usually bear artistic and historical relevance, but tracing their origins using traditional surface measurement systems can be difficult if the artistic work is installed and immobile.
Implementing PMD techniques on consumer devices using **webrtc-perception** is an alternative way to measure the surface patterns by instead "scanning" the glass with the mobile device.

<div class="feature-image">
    <a href="{{ site.url }}/{{ site.assets.features }}/{{ page.specifics.images }}/03_pmd_slide_1.png">
        <img src="{{ site.url }}/{{ site.assets.features }}/{{ page.specifics.images }}/03_pmd_slide_1.png" width="800" style="padding:0px 0px 0px 0px;">
    </a>
</div>

The device used for data capture was again an **NVIDIA SHIELD K1** tablet.
Florian's application used **webrtc-perception** to access the front-facing camera on a device and change camera settings for the connected client.
When paired with some JavaScript I wrote for generating sinusoidal patterns on the K1's display, he can generate any number of periodic image patterns on the display, use WebRTC to record image captures of the reflected pattern, transmit them to the processing server, and see the phase mapping results in real-time.
Adjusting the projected light patterns requires some JavaScript and trigonometric acumen on the developers' part, but the client merely needs to reload the webpage hosting the `rtc-deflectometry` content to get updated JavaScript code, and tweaks to server processing code are invisible to the client device.

`rtc-deflectometry` was demonstrated on the Kokomo sample glass tiles, on decorative pieces we acquired for measurement purposes, and on various other objects (_even those not strictly made of glass_) that exhibit specular reflection.
Our results and a description of the work was **[featured in Optics Express Vol. 28, Issue 7](https://www.osapublishing.org/oe/abstract.cfm?uri=oe-28-7-9027)** in March 2020, and the patent for this work was granted for this particular combined integration of PMD and mobile devices not long afterwards.

I even got to do a bit of hand modeling for the feature's preview image!

<div class="feature-image">
    <a href="{{ site.url }}/{{ site.assets.features }}/{{ page.specifics.images }}/04_hand_model.png">
        <img src="{{ site.url }}/{{ site.assets.features }}/{{ page.specifics.images }}/04_hand_model.png" width="480" style="padding:0px 0px 0px 0px;">
    </a>
</div>

<hr>

##### Attribution

_Special thanks to the NU Computational Photography Lab for the screenshot of Kai's work currently serving as the project thumbnail._
