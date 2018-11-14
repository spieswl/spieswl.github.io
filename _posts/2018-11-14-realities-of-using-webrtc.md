---
layout:             post
title:              "(Painful) Realities of Using MediaCapture and WebRTC"
description:        "My recent experiences with WebRTC and its APIs have given me a chance to get up close and personal with its capabilities and limitations. While its basic functions work as advertised, advanced uses of MediaCapture are far tougher to get working properly. In many cases, features are not supported by particular browsers, implementations are inconsistent across devices, or controls simply have not yet been programmed."
keywords:           webrtc, mediacapture, javascript, web development, computer vision, safari, chrome, android, iOS, mobile
tags:               [WebRTC, JavaScript, Mobile Development, Computer Vision, Retrospectives]

folders:
  images:           "webrtc-realities"                      # This path is post-dependent; don't forget to change it!

published:          true
---

In [the project entry describing my most recent work with **WebRTC**]({{site.url}}/projects/2018/webrtc-and-computational-photography), I covered a number of _MediaCapture_ features I am using to implement an adaptable image capture system for my colleagues. The project write-up focuses on motivations, implementation for specific applications, potential extensions of the work...the type of information that demonstrates the potential of a technology. This post, on the other hand, exists so that I may speak, at length, about my frustrations with using WebRTC to create the tool in the first place. I am venting more than I would normally do in such a post, but I have spent a lot of time studying these APIs, and the development surrounding them, and the way devices respond to them...and it has been immensely frustrating at points.

First, a recap. To briefly paraphrase the [W3C technical report](https://www.w3.org/TR/webrtc/), **WebRTC** is a set of ECMAScript (_JavaScript, for the less standards inclined_) APIs that facilitate the transmission/reception of media content from another browser or device implementing the appropriate protocols. It does this in conjunction with another, connected API specification that handles the accessing of cameras and microphones on client hardware, otherwise known as **MediaStream**. **getUserMedia()** is actually a component of the **MediaDevices** object, which itself is a part of the _MediaStream_ API, but I digress. _getUserMedia()_ returns information about a content stream from a client device. This split is important to note: although _WebRTC_ deals with the logistics, _MediaStream_ fetches the source device details, and _MediaCapture_ provides control over the capture environment, the cluster are commonly referred to as a group by the _WebRTC_ moniker. This relationship between these APIs ensures that most (not all!) of the time that _WebRTC_ is in use, _MediaCapture_ or _MediaStream_ is also in use. There are situations where one API is used and not the others, and vice versa, but the application described in the **`webrtc-perception`** entry does indeed use all three. 

_**A disclaimer**_: I am not proficient in `JavaScript`, so any of the code snippets I highlight here are likely to have shortcomings obvious to veteran web developers or JavaScript programmers. I do not believe any of my complaints are as a result of shoddy code, though it is possible. That said, most challenges along the way have been successfully overcome by studying the documentation, testing my code, tweaking my approach...doing things that engineers do in order to overcome technical challenges. I also suspect that a number of my problems with WebRTC are a result of using features rarely demanded or leveraged in this way.

Throughout the process of writing _webrtc-perception_, I have faced continuous challenges to making something that is both broadly usable and powerful. Devices with inconsistent camera control implementations? _**Check.**_ Safari not supporting advanced camera controls? _**Check.**_ Chrome on iOS being fundamentally different than everywhere else, leading to a hamstrung WebRTC implementation? _**Check.**_ Being unable to "stringify" a MediaSettingsRange object, so I have to hack that in myself? _**Check.**_ The hits just keep coming. The last issue is arguably a nitpick and part of overcoming simple challenges on the job, but the end result still looks and feels like an unnecessary hack that works against the turnkey nature of the application I am trying to build. I am sure there will be more issues that eventually crop up, but I want to focus on the first few problems due to the existential nature they pose to the _webrtc-perception_ toolbox. 

1. Gaps in the _MediaCapture_ Constraints Interface
2. Advanced _MediaCapture_ API Components are Unsupported Everywhere but Chrome
3. Other Browsers on iOS Support a Limited WebRTC Implementation
4. Inconsistent Implementations of Constraint Application on the Device Level


### Gaps in the _MediaCapture_ Constraints Interface

As computer vision systems and other applications push for more control over their component cameras for various reasons, having the ability to place constraints on the operation of these cameras grows in priority. For CV applications that have low-level access to machine vision cameras, or native apps on smart phones, much work has already been done to ensure consistent and comprehensive interfaces exist for programmers to use. On the _**Orthrus**_ project, for example, my `C++` code uses a Point Grey Research-supplied library for working with their cameras that can be objectified and used to great effect by motivated programmers. In the case of WebRTC, we are provided a similar, simple interface in the _MediaStream Image Capture_ API. The W3C documentation specifically mentions constrainable properties that, when modified, can be used to change the operation of a camera device immediately. If you'll note the following chart (_pulled from [Section 9.2](https://www.w3.org/TR/image-capture/#mediatrackcapabilities-section) in the W3C Working Draft_), the specification includes a lot of constraints accessible by a programmer!

<div class="project-image">
    <img src="{{ site.url }}/{{ site.post_assets }}/{{ page.folders.images }}/mediatrack_capabilities.png" style="width:822px">
</div>

That's a good start! But, watch out, **focusDistance** is not officially supported by any browser yet (so it CANNOT be used) and **exposureTime** is not listed anywhere (more on that in a minute), except in the [editors' draft](https://w3c.github.io/mediacapture-image/). The maintainers must have realized that **exposureCompensation** was not sufficient for true exposure control and added the time constraint sometime between June 2017 and November 2018. This is all well and good, but focus distance and exposure time control are fairly critical for any photographic applications...and they have been unimplemented in the API for over a year and a half! You can change **whiteBalance** but you cannot change **exposureTime**. I was initially surprised and thought it to be an error on my end, as this seems like a huge oversight. Confirmation from a [Chromium Google group discussion thread](https://groups.google.com/a/chromium.org/forum/#!topic/blink-dev/oNxzXaFY9c8) ([and the linked feature tracker](https://www.chromestatus.com/feature/5181454237564928)) confirms the work-in-progress state for **focusDistance**, and [here](https://bugs.chromium.org/p/chromium/issues/detail?id=823316) for **exposureTime**. As the measurement modalities require some amount of focus and exposure control in order to function properly, we are at the mercy of the WebRTC developers and maintainers.

### Advanced _MediaCapture_ API Components are Unsupported Everywhere but Chrome

The chart below (drawn from [this page](https://w3c.github.io/web-roadmaps/media/capture.html)) says quite a bit about the adoption rate of WebRTC and MediaCapture features.

<div class="project-image">
    <img src="{{ site.url }}/{{ site.post_assets }}/{{ page.folders.images }}/webrtc_feature_chart.png" style="width:790px">
</div>

Unfortunately, it shows just how much Apple lags in making their devices work with more sophisticated aspects of WebRTC. They [shipped support for WebRTC in WebKit](https://webkit.org/blog/7726/announcing-webrtc-and-media-capture/) back in June of 2017, but as the chart shows, you cannot do much of anything in Safari...and as [this article from _webrtcH4cKS_](https://webrtchacks.com/guide-to-safari-webrtc/) so clearly lays out, you're effectively limited to the most simple of uses.

I am not sure how to gauge Apple's lack of progress or relative quiet. I would guess they are far more interested in keeping programmers that need sophisticated camera controls to do something contained to the inside of an iOS app. This is a dead block, and effectively eliminates an entire segment of the mobile device carrying populace from using anything in _webrtc-perception_.

To Mozilla's credit, they are currently in development on a number of these APIs, but their limited mobile footprint makes it difficult to take their support as a good sign for _webrtc-perception_'s use on mobile devices.

### Other Browsers on iOS Support a Limited WebRTC Implementation

In an attempt to find something that **_might_** work on an Apple device, I started looking at Chrome on iOS. The thought process was that if Apple did not have advanced feature support in Safari, Chrome on iOS might be able to supply the answers we were looking for.

As an aside, the reason I spent so much time chasing down Apple device support was that we expected camera device driver support (as well as the hardware itself) to be comparatively high quality among all mobile devices...and not subject to phone manufacturer implementation quirks (_more on that in the next section_) when handling constrainable settings like **exposureTime**, **whiteBalance**, **zoom** control, etc.

It turns out that, as Chrome on iOS uses `WKWebView` (and all in-app browsers follow that trend) and `WKWebView` is [not capable of using WebRTC](https://bugs.chromium.org/p/chromium/issues/detail?id=752458) to the degree we need, this is also an untenable scenario for _webrtc-perception_.

<hr>

**INTERMISSION:** To recap, we are currently stuck with partial support on Safari on an Apple device. We have NO support on any other browser on an Apple device. To top that off, on Android devices, advanced photography features are only partially implemented in _MediaCapture_. That I know of, there is one more major issue to consider.

### Inconsistent Implementations of Constraint Application on the Device Level

This is a _fun_ one, and I am also still trying to determine the scope of this issue. I did not do an exhaustive study of the behavior of the [NVIDIA SHIELD K1](https://en.wikipedia.org/wiki/Shield_Tablet) tablet we have been using for the **`rtc-deflectometry`** application, but the few observations we have made up to this point lead me to believe that certain constraints (most easily observed with **whiteBalance** on the K1) just outright do not work like a programmer would expect.

A picture of the controls with certain constrainable settings activated is shown below, and further down is the code that is executed when the remote device receives the desired settings from the host device.

<div class="project-image">
    <img src="{{ site.url }}/{{ site.post_assets }}/{{ page.folders.images }}/constraint_settings.png" style="width:800px">
</div>


```javascript
function applyNewConstraintsFromRemote(constraints)
{
    let track = localStream.getVideoTracks()[0];

    track.applyConstraints(constraints).then(function()
    {
        getStreamFeedback(localStream);

        socket.emit('apply_response', true);
    })
    .catch(function(error)
    {
        handleError(error)
        socket.emit('apply_response', false);
    });
}
```

Essentially, the range bars and radio buttons are queried when hitting a (not pictured) **Apply Constraints** button further up the page. Those are formatted in a way that _WebRTC_ is expecting and transmitted to the other device. In terms of the code I use to apply a constraints package received from another device, this is it. Very simple, fairly clean.

So check out this sequence of camera device behaviors when I try to establish manual **whiteBalance** control and change the **colorTemperature** constraint, which should have a noticeable effect on our live camera feed. The white balance should, if the constraint is working properly, track values displayed on the the [color temperature chart here on Wikipedia](https://en.wikipedia.org/wiki/Color_temperature). Starting out from _automatic_, the camera [looks to be configured around 6000K]({{ site.url }}/{{ site.post_assets }}/{{ page.folders.images }}/wb_test_s0.png). Note that the capabilities of the forward-facing camera on the SHIELD K1 dictate that the color temperature can range from 2850K up to 7000K, so our settings have to lie somewhere between those values. Also note that these actions were taken exactly in this order:

1. Set **whiteBalance** to _manual_, set **colorTemperature** to _2850K_. [Result]({{ site.url }}/{{ site.post_assets }}/{{ page.folders.images }}/wb_test_s1.png) -> Displayed image shifts to displaying warmer tones, estimated to be somewhere around 3000K. 
2. Set **whiteBalance** to _automatic_, set **colorTemperature** to any random value (color temperature is not passed when setting an automatic **whiteBalance** mode in my code). [Result]({{ site.url }}/{{ site.post_assets }}/{{ page.folders.images }}/wb_test_s2.png) -> Camera converts back to automatic white balancing, which I estimate to be somewhere around 6000-7000K. Curiously, this seems to be a different color temperature than what we started with.
3. Set **whiteBalance** to _manual_, set **colorTemperature** to _7000K_. [Result]({{ site.url }}/{{ site.post_assets }}/{{ page.folders.images }}/wb_test_s3.png) -> No change. This is where the functionality begins to fall apart.
4. Set **whiteBalance** to _automatic_, leave **colorTemperature** set to _7000K_. [Result]({{ site.url }}/{{ site.post_assets }}/{{ page.folders.images }}/wb_test_s4.png) -> The camera white balance is now even warmer than the results from the first step; I estimate around 2700K (possibly right at 2850K).
5. Set **whiteBalance** to _automatic_, set **colorTemperature** to _4000K_. [Result]({{ site.url }}/{{ site.post_assets }}/{{ page.folders.images }}/wb_test_s5.png) -> No change from prior step.
6. Set **whiteBalance** to _manual_, leave **colorTemperature** set to _4000K_. [Result]({{ site.url }}/{{ site.post_assets }}/{{ page.folders.images }}/wb_test_s6.png) -> No change from prior step.
7. Set **whiteBalance** to _automatic_, set **colorTemperature** back to _2850K_. [Result]({{ site.url }}/{{ site.post_assets }}/{{ page.folders.images }}/wb_test_s7.png) -> Camera seems to have automatically selected a color temperature somewhere around the 4000-5000K range.

As you can see, there is little rhyme or reason to the applied constraints and their effect on the video track; if there is a rhyme, it seems like there might be a strange two-step approach to converting to manual control and then setting the desired color temperature. I can confirm the constraints are being properly formed and handled by the device, as error handling will normally display a browser alert popup detailing any issues if we end up submitting malformed constraints via the _MediaCapture_ API.

In another example, on my **OnePlus 3T** Android phone, I noticed that the rear-facing camera allows the **torch** setting to be changed. Enabling the torch does, in fact, turn on the rear LED when applying that constraint to the phone, but I cannot turn the LED off without locking the phone screen, which automatically deactivates the torch until I go to apply the constraint again. The SHIELD K1 does not have a rear-facing LED to go with the rear-facing camera, so that setting is (rightly) unavailable for that track. Attempting to set the boolean constraint for the torch to `false` does nothing, even though setting it to `true` and applying that constraint is what turns on the LED in the first place.

After all that, you might understand why I am personally frustrated with WebRTC, especially as it has incredible potential as a powerful, easy-to-access camera control (and microphone control!) platform. In terms of being able to facilitate all of the needs of **`rtc-deflectometry`** and **`rtc-shapeshifter`**, perhaps one day, hopefully soon, _webrtc-perception_ can seamlessly support those applications across all mobile devices. In the meantime, if you are looking to commission a framework with similar aims, _**please**_ take these discoveries and observations into account. I sincerely hope you have more luck than I have had.

If you would like to chat with me about WebRTC (and my experiences with it), don't be afraid to reach out to me via the Administrator contact info on the **[About]({{site.url}}/about)** page.
