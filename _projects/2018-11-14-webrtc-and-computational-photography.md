---
layout:             project
title:              "webrtc-perception : Using WebRTC for Computational Photography"
date:               "2018-11-14"

description:        "Devices capable of providing images for computational applications are abundant, but, outside of mobile app 'walled gardens', few frameworks are freely available for academic, artistic, or scientific use. This project contains examples of using WebRTC for surface reconstruction, surface measurement, and near-real-time modeling with consumer-accessible tablets or cell phones."
keywords:           webrtc, javascript, computer vision, web development, computational photography, image processing, photometric stereo, phase measured deflectometry
tags:               [WebRTC, JavaScript, Python, Computer Vision, Deflectometry, Photometric Stereo]

folders:
  images:           "webrtc-perception"                     # This path is project-dependent; don't forget to change it!

published:          true
---

During my time spent in Northwestern University's **[Computational Photography Lab](http://compphotolab.northwestern.edu/)**, I divided my attention between the 3D Scanner project [described here]({{site.url}}/projects/2018/single-shot-structured-light-depth-scanning) and another project centered around **[WebRTC](https://webrtc.org/)**. WebRTC (_RTC stands for Real-Time Communications_) is a suite of APIs that enables the capture and transfer of video and audio content entirely through a web browser. Several applications and products already leverage WebRTC for video conferencing, gaming, media sharing, and other social applications, so it has benefited from steady growth and support since its introduction at the 2013 Google I/O developers conference. A select few have also used WebRTC to facilitate IoT applications, serve as the framework for hobbyist projects, and have integrated it into cutting-edge computer science and robotics research.

I started looking at _WebRTC_ APIs in mid-2018 to determine if our lab could use such a technology as the basis for a scientific data collection system. The aim was to develop an image capture framework that could be immediately usable for multiple ongoing research projects without requiring the researchers to possess special hardware or be familiar with the nuances of WebRTC. Our lab also looked at _WebRTC_ as a chance to create a system that could eventually be used by individuals outside of our laboratory environment, such as art curators and conservators, for historical or scientific documentation purposes. The imposing limitation that the end system cannot require users to download a separate application, and instead only use browser-supplied capabilities, does constrain the potential capabilities somewhat, but also allows for a wider audience and subsequently broader use.

The design of **`webrtc-perception`** includes a capture website, a dedicated server for processing image data, and a results display website. Since WebRTC is used for capture and transport, users need to rely on other resources to complete their application, such as a dedicated server to handle image and data processing tasks and return useful results. This also confers some advantages, as operators can improve the processing code on the fly, change camera controls and presentation details on the respective websites, and fix issues without users needing to download or install any new files or update applications.

This project also leans on another library named `aiortc` to implement Python-based interaction with connecting clients via WebRTC and perform useful computation on images and other data gathered through the use of WebRTC. Jeremy Lainé has put together a very useful package and I highly recommend [giving it a closer look](https://github.com/jlaine/aiortc) if you are the least bit interested in this framework!

A barebones illustration of the _webrtc-perception_ framework is shown in the following graphic. This will give you an idea of what an end-to-end system _could_ look like, but without the _rtc-shapeshifter_- or _rtc-deflectometry_-specific details included.

<div class="project-image">
    <img src="https://raw.githubusercontent.com/spieswl/webrtc-perception/master/docs/webrtc-perception_block_diagram.png" style="width:800px">
</div>

One more thing: if you are intending to re-implement portions of this project or are otherwise unfamiliar with the implementation details of WebRTC, I encourage you to investigate the videos and guide content over at [https://webrtc.org/start/](https://webrtc.org/start/). There are plenty of resources to get people unfamiliar with WebRTC caught up to speed. Take particular note of `MediaStream` and `RTCDataChannel` details, as our tools make use of those two extensively.

### webrtc-perception

The project "metapackage" is named **webrtc-perception** and is hosted over at GitHub at [https://github.com/spieswl/webrtc-perception](https://github.com/spieswl/webrtc-perception). The application-specific code is contained within the "content" folder, while the metapackage serves as a catch-all issue tracker and documentation holder that we can collectively address.

Speaking more specifically than above, _webrtc-perception_ uses WebRTC to establish a connection between the server and the client device in a seamless manner. `getUserMedia()` and other _MediaStream_ components simplify connecting to a client device, such as an integrated camrea. The client device, thanks to `MediaStream` features, also allows the server to decide which photography settings to use for that particular camera (such as _**exposure time**_, _**ISO**_, _**white balance**_, _**focus distance_**, rear _**torch**_ status, etc.) for a particular application. The client signals to the server when it is ready to begin data capture, and the server responds with a signal to start "measuring" with the device. The server handles gathering data from the client and performing application-specific computation on the gathered data. My server architecture uses Python and `aiortc` to connect with a client via WebRTC without needing to use a web browser. The Python code also converts the results of the computation into a format which can be transmitted to another, separate website designed to display (and make available, if necessary) the results. My system attempts to do this as close to real-time as possible, so that the user in control of the measurement client can evaluate the measurement process in something akin to a feedback loop.

At present, two sub-projects currently make their home in the metapackage: **`rtc-shapeshifter`** and **`rtc-deflectometry`**. Each sub-project is tied to active research in the **Computational Photography Lab**. The next couple sections outline the aspirations of each sub-project and how my colleagues are using **webrtc-perception** to achieve those aims.

#### > rtc-shapeshifter

**`rtc-shapeshifter`** is a _WebRTC_-based tool that expands upon a concept originally presented by one of my colleagues [Chia-Kai Yeh](https://github.com/kaiyeh0913) called "[Shape by Shifting](https://ieeexplore.ieee.org/abstract/document/8109194)". His work originally used DSLR cameras to get early results and switched to using an iPhone (with some special hardware) in its later configuration, which made it a prime candidate for a measurement technique that could be extended by _webrtc-perception_. While this entry will not go into deep technical detail on his work, I have included some slides from a presentation we held for a technical interest group here at Northwestern University on October 19th:

<div class="project-image">
    <a href="{{ site.url }}/{{ site.project_assets }}/{{ page.folders.images }}/sfs_slide_1.png">
        <img src="{{ site.url }}/{{ site.project_assets }}/{{ page.folders.images }}/sfs_slide_1.png" style="width:600px; padding:0px 0px 0px 0px;">
    </a>
    <a href="{{ site.url }}/{{ site.project_assets }}/{{ page.folders.images }}/sfs_slide_2.png">
        <img src="{{ site.url }}/{{ site.project_assets }}/{{ page.folders.images }}/sfs_slide_2.png" style="width:600px; padding:0px 0px 0px 0px;">
    </a>
</div>

In short, Kai is using the _webrtc-perception_ framework to make it easier for him to recover surface normal maps with an off-the-shelf smartphone by way of photometric stereo. He can control various photography settings remotely, trigger image capture from the rear-facing camera (_with the LED light enabled_), clip on his polarizer, and automate processing and results generation...and see his results _while_ capturing data.

<!-- TODO : Results screencap goes here -->

#### > rtc-deflectometry

**`rtc-deflectometry`** is a WebRTC-based tool that implements _**Phase Measuring Deflectometry (PMD)**_ in order to optically measure partially specular surfaces. In particular, Dr. Florian Willomitzer, the leading CPL post-doc, was eager to measure some special glass tiles that we have in the lab. These glass tiles have a particular surface shape that bears some historical relevance, so implementing PMD techniques on consumer devices using WebRTC to try and recover the surface shape is the goal of this sub-project. PMD, for those who are unfamiliar, can be described as using light projected in a periodic pattern along with an observing camera element to perceive how a surface deforms the observation of the pattern.

<div class="project-image">
    <a href="{{ site.url }}/{{ site.project_assets }}/{{ page.folders.images }}/pmd_slide_1.png">
        <img src="{{ site.url }}/{{ site.project_assets }}/{{ page.folders.images }}/pmd_slide_1.png" style="width:800px; padding:0px 0px 0px 0px;">
    </a>
</div>

Florian's application uses _webrtc-perception_ to access the front-facing camera on a device and allow for his application to change camera settings on the connected client. When paired with JavaScript code I wrote for generating sinusoidal patterns, he can display a periodic image pattern in the device display, use WebRTC to record image captures of the morphed pattern, transmit them to the processing server, and see the phase map results in real-time. The changing of light patterns requires some JavaScript acumen, but the client merely needs to reload the web interface to get updated JavaScript code, and tweaks to server processing code are invisible to the client device.

<!-- TODO : Results screencap goes here -->

More details on the device calibration, our results for both `rtc-shapeshifter` and `rtc-deflectometry`, and ideas for extensions to our work can be found in the paper on **[webrtc-perception]()**, which is coming soon.

<hr>

On a closing note, `JavaScript` is not my programming language of choice and I do not usually take on projects with a heavy web development focus. Instead, the opportunity to provide not only just a single computer vision tool, but a complete computer vision toolbox for my fellow researchers was appealing enough that I was eager to contribute to the development of **webrtc-perception**. The fact that this project has immediate application to ongoing research and WebRTC itself provided a straightforward path to creating an end-to-end scientific tool was also a notable bonus. All things considered, I do not see myself moving away from working in `C++` or `Python` anytime soon.

Do not miss checking the **[About]({{site.url}}/about)** page for my most recent contact information if you want to talk about this work, or any other content!
