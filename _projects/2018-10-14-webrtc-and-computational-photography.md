---
layout:             project
title:              "Using WebRTC for Computational Photography"
date:               "2018-10-14"

description:        "Devices capable of providing images for computational applications are abundant, but, outside of mobile app 'walled gardens', few frameworks are freely available for academic, artistic, or scientific use. This project contains a few examples of using WebRTC for surface reconstruction, measurement, or near-real-time modeling applications with consumer-grade tablets or cell phones."
keywords:           webrtc, javascript, computer vision, web development, computational photography, image processing, structure from motion
tags:               [WebRTC, JavaScript, Computer Vision, Deflectometry, Structure from Motion]

folders:
  images:           "webrtc-perception"                     # This path is project-dependent; don't forget to change it!

published:          false
---

During my time spent in Northwestern University's **[Computational Photography Lab](http://compphotolab.northwestern.edu/)**, I divided my attention between the 3D Scanner project [described here]({{site.url}}/projects/2018/single-shot-structured-light-depth-scanning) and another project centered around **[WebRTC](https://webrtc.org/)**. WebRTC (_where RTC stands for Real-Time Communications_) is a suite of APIs that enables the capture and transfer of video and audio content entirely through the browser. Several applications and products also leverage WebRTC's native code options for video conferencing, gaming, media sharing, and other social applications, so it has enjoyed steady growth and support since its introduction in 2013 by Google. A select few have also used WebRTC to facilitate IoT applications, serve as the framework for hobbyist projects, and have integrated its use into cutting-edge computer science and robotics research.

For our needs, I started looking at WebRTC in mid-2018 to determine if our lab could use such a technology as the basis for a general purpose data collection system. The aim was to develop something that could be immediately usable for multiple ongoing research projects without requiring the users to be familiar with the nuances of WebRTC. We also looked at WebRTC as a chance to work on a system that could eventually be used by individuals outside of our lab, such as art curators and conservators, for historical or scientific purposes.

The components of our system, at a minimum, include a capture website, a dedicated processing server, and a results display website. Since WebRTC is only for capture and transport, we needed to rely on a dedicated server to handle the processing and return usable results. This also confers some advantages, as we could improve the processing code on the fly, change camera controls and presentation details on the respective websites, and do so without users needing to download or install any new files or applications.

This project makes use of another library named `aiortc` to implement Python-based interaction with other clients via WebRTC and perform useful computation on images and other data gathered through the use of WebRTC. Jeremy Lainé has put together a very useful package and I highly recommend [giving it a closer look](https://github.com/jlaine/aiortc) if you are the least bit interested in WebRTC!

A barebones illustration of our **`webrtc-perception`** framework is shown in the following graphic. This will give you an idea of what an end-to-end system _could_ look like, but without our _shapeshifter_- or _deflectometry_-specific details included.

<div class="project-image">
    <img src="https://raw.githubusercontent.com/spieswl/webrtc-perception/master/docs/webrtc-perception_block_diagram.png" style="width:800px">
</div>

One more thing: if you are intending to re-implement portions of this project or are otherwise unfamiliar with the implementation details of WebRTC, I encourage you to investigate the videos and guide content over at [https://webrtc.org/start/](https://webrtc.org/start/). There are plenty of resources to get people unfamiliar with WebRTC caught up to speed. Take particular note of `MediaStream` and `RTCDataChannel` details, as our tools make use of those two extensively.

### webrtc-perception

The project "metapackage" is named **webrtc-perception** and is hosted over at GitHub at [https://github.com/spieswl/webrtc-perception](https://github.com/spieswl/webrtc-perception). The application-specific code is contained within the "content" folder, while the metapackage serves as a catch-all issue tracker and documentation holder that we are keeping up to date.

At present, two sub-projects currently make their home in **webrtc-perception**: **`rtc-shapeshifter`** and **`rtc-deflectometry`**. Each sub-project is tied to active research in the **Computational Photography Lab**; the next few sections outline the aspirations of each sub-project and how we are using WebRTC to achieve those aims.

#### > rtc-shapeshifter

`rtc-shapeshifter` is a WebRTC-based tool that expands upon a concept originally presented by one of my colleagues [Chia-Kai Yeh](https://github.com/kaiyeh0913) called "[Shape by Shifting]()".

<!--
TODO : Technical details on Shape-by-Shifting as stated by Kai
TODO : Architectural diagram
TODO : Results!
-->

#### > rtc-deflectometry

`rtc-deflectometry` is a WebRTC-based tool that implements _**Phase Measuring Deflectometry**_ in order to optically measure partially specular surfaces. In particular, we were eager to measure specialized glass tiles that we keep around the lab. These glass tiles have a particular surface shape to them that may have some historical relevance, so implementing PMD techniques on consumer devices using WebRTC to try and recover the surface shape is the goal of this sub-project.

<!--
TODO : Technical details on the PMD setup from Florian W.
TODO : Architectural diagram
TODO : Results!
-->

<hr>

On a final note, `JavaScript` is not my programming language of choice and I do not usually take on projects with a heavy web development focus. Instead, the opportunity to provide not only a single computer vision tool, but a complete computer vision toolbox was appealing enough that I was eager to contribute to the development of **`webrtc-perception`**. The fact that it had immediate application to ongoing research and provided a straightforward path to creating an end-to-end scientific tool was also a notable bonus. All things considered, however, I do not see myself moving away from working in `C++` or `Python` anytime soon.