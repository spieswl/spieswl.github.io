---
layout:             post
title:              "Realities of Using MediaStreams and WebRTC"

description:        "My recent experiences with using WebRTC in a mobile application gave me a chance to get familiar with its capabilities and limitations, namely being reliant on other protocols to acquire media data. While basic MediaStream functions work as advertised, advanced uses are far tougher to get working consistently. In many cases, features are not supported by particular browsers, implementations are inconsistent across devices, or controls simply have not yet been implemented."
keywords:           webrtc, mediastream, computer vision, safari, firefox, chrome, android, iOS, mobile devices
tags:               [Retrospectives, WebRTC, Computer Vision]

specifics:
    noteworthy:     false
    images:         "2018/webrtc-realities"

published:          true
---

In [the project entry describing my work with WebRTC]({{site.url}}/projects/2018/webrtc-and-computational-photography), I covered a number of API features that I am using to implement an adaptable image capture and analysis system connected to a portable, mobile device.
The project write-up focuses on the motivations, implementation tweaks for particular measurement techniques, and potential extensions of the work--the type of information that demonstrates the potential of this technology beyond its stated focus as a communications framework.
This post, on the other hand, exists so that I can outline a few of the frustrations I endured when using **_MediaStream_** and **_WebRTC_** APIs to support those goals.
I have spent a good portion of the past few weeks studying the _WebRTC_ and _MediaCapture_ specifications, some of the correspondence surrounding their development, and the way devices should and do respond when using their implementations.
To put it plainly, it has been immensely challenging to get a consistent experience.

First, a recap. To briefly paraphrase the [W3C technical report](https://www.w3.org/TR/webrtc/), **WebRTC** is a set of ECMAScript APIs that facilitate the transmission and reception of media content from another browser or device implementing the appropriate protocols.
It does this in conjunction with another connected API specification that handles the accessing of media sources on client hardware, otherwise known as **MediaStream**.
One method of particular note, `getUserMedia()`, is actually part of the interface to a _MediaDevices_ object, which is itself a part of the higher-level _MediaStream_ API.
`getUserMedia()` returns information about a content stream from a client device, which is assumed to have some kind of camera and/or microphone.
Other methods enumerate the source device details and provide control over the capture environment, generally through applying _constraints_.
The relationship between capturing and relaying media data ensures that most of the time that _WebRTC_ is used, _MediaStream_ is also being used.
There are situations where one API is used and not the others, and vice versa, but the application samples maintained in the `webrtc-perception` repository do indeed use both.

_**A quick disclaimer**_: I am not a proficient JavaScript developer, but I suspect that most of my problems with _MediaStream_ APIs are actually a result of using features rarely leveraged or handled in the ways needed for computational photography work.
These APIs may be working well for typical use-cases, but I cannot attest to that here.
Your mileage may vary.

Throughout the creation of `webrtc-perception`, my intention was to have an implementation that was both flexible in terms of supported devices and powerful in terms of device reconfiguration.
Unfortunately, that intention was dogged by a number of issues both with the consumer devices themselves and with the browsers running on them.
Devices with inconsistent camera controls?
**_Check. Finding a tablet that allowed us to get the granular camera control needed was exceedingly difficult._**
Safari not supporting advanced camera controls?
**_Check. This ultimately prevented us from using Apple devices which we believed to have the best camera hardware available in a mobile device._**
Chrome on iOS being fundamentally different than everywhere else, resulting in a hamstrung _WebRTC_ implementation in addition to the former issue?
**_Check._**
Being unable to consistently handle a _MediaSettingsRange_ object, leading to a need for custom code just to pass around arguments to a standard interface? **_Check._**
The hits just kept coming when trying to get the system working.
That last issue is arguably a nitpick and can be found in the DNA of most software projects, but the end result still looks and feels like an unnecessary hack that works against the turnkey nature of `webrtc-perception` and using _WebRTC_ itself. 

I want to focus on the first few problems due to the existential nature they pose to creating a widely-accessible mobile device measurement system.

1. Gaps in the _MediaStream_ Constraints Interface
2. Advanced _MediaStream_ API Components are Unsupported Everywhere but Chrome
3. Other Browsers on iOS Support a Limited _WebRTC_ Implementation
4. Inconsistent Implementations of Constraint Application on the Device Level

<hr>

### Gaps in the _MediaStream_ Constraints Interface

As computer vision applications generally need a greater degree of control over camera settings, having the ability to place constraints on a camera's operational configuration is crucial.
For CV applications that have been integrated with machine vision cameras (as in industrial environments), or that are using native applicatons on smart phones, much work has already been done to ensure consistent and comprehensive camera control interfaces exist.
On the other project I was working on for the **CPL**, my first task was to use camera-vendor-supplied `C++` libraries to create an easy-to-use camera control interface.
Only then could we use the special cameras in a way consistent with the requirements of that particular application.
In the case of _WebRTC_, users are provided a similar type of interface in the _MediaStream Image Capture_ API.
The W3C documentation specifically mentions constrainable properties that, when modified, can be used to change the settings of a camera feed immediately.
If you'll note the following chart (_pulled from [Section 9.2](https://www.w3.org/TR/image-capture/#mediatrackcapabilities-section) in the W3C Working Draft_), the specification includes a lot of important settings that are supposed to be accessible by a developer.

<div class="post-image">
    <a href="{{ site.url }}/{{ site.assets.posts }}/{{ page.specifics.images }}/01_mediatrack_capabilities.png">
        <img src="{{ site.url }}/{{ site.assets.posts }}/{{ page.specifics.images }}/01_mediatrack_capabilities.png" width="822">
    </a>
</div>

That is a great start!
But watch out, **focusDistance** is not officially supported by any browser as of this writing in 2018 (_so it cannot be used_) and **exposureTime** is not listed anywhere (_more on that in a minute_), except in the [editors' draft](https://w3c.github.io/mediacapture-image/).
The maintainers must have realized that **exposureCompensation** was not sufficient for true exposure control and added the time constraint sometime between June 2017 and November 2018.
This is all well and good, but focus distance and exposure time control are fairly important for any advanced photographic application.
It is possible to change **whiteBalance** but you cannot change **exposureTime**.
You may be able to figure out an acceptable **exposureCompensation** or **ISO** value, though those are not truly replacements for the lack of other controls.
I was initially surprised and thought it to be an error on my end, as this seems like a huge oversight, even if the limitations in control arise because of hardware choices on the part of device manufacturers.
Confirmation from a [Chromium Google group discussion thread](https://groups.google.com/a/chromium.org/forum/#!topic/blink-dev/oNxzXaFY9c8) ([and the linked feature tracker](https://www.chromestatus.com/feature/5181454237564928)) confirms the work-in-progress state for **focusDistance**, and [here](https://bugs.chromium.org/p/chromium/issues/detail?id=823316) for **exposureTime**.
As the measurement methods my colleagues look to leverage require some amount of focus and exposure control, we are at the mercy of standards developers, maintainers, and device manufacturers.
Since we were unwilling to wait for some indeterminate amount of time for developments on this front, I instead looked for mobile devices that had camera systems that could work well without needing too much adjustment.

<hr>

### Advanced _MediaStream_ API Components are Unsupported Everywhere but Chrome

The chart below (saved here in late 2018 from [this page](https://w3c.github.io/web-roadmaps/media/capture.html)) says quite a bit about the adoption rate of _WebRTC_ and _MediaStream_ features.

<div class="post-image">
    <a href="{{ site.url }}/{{ site.assets.posts }}/{{ page.specifics.images }}/02_webrtc_feature_chart.png">
        <img src="{{ site.url }}/{{ site.assets.posts }}/{{ page.specifics.images }}/02_webrtc_feature_chart.png" width="790">
    </a>
</div>

Unfortunately, it shows just how much Apple lags in making their devices work with more sophisticated aspects of _WebRTC_.
They [shipped support for WebRTC in WebKit](https://webkit.org/blog/7726/announcing-webrtc-and-media-capture/) in mid-2017, but as the chart shows, you cannot do much of anything in Safari...and as [this article from _webrtcH4cKS_](https://webrtchacks.com/guide-to-safari-webrtc/) lays out, you are effectively limited to the most simplistic of uses.

I am not sure how to gauge Apple's lack of progress and overall silence.
I would guess they are far more interested in keeping developers that need sophisticated camera control restricted to the insides of an iOS app.
For our use case, this is a huge blocker and effectively eliminates an entire segment of the mobile device domain from being useful for anything in `webrtc-perception`.

To Mozilla's credit, they are currently in development on a number of these APIs, but their limited mobile development influence makes it difficult to take their support as a good sign for future adoption on mobile devices (_and, ultimately, broader use of_ `webrtc-perception`).

<hr>

### Other Browsers on iOS Support a Limited _WebRTC_ Implementation

In an attempt to find something that **_might_** work on an Apple device, I started looking at Chrome on iOS.
The thought process was that if Apple did not have advanced feature support in Safari, Chrome on iOS might instead be able to supply the control we needed.
As an aside, the reason I spent so much time chasing down Apple device support was that we expected camera device driver support, as well as the hardware itself, to be of the highest quality among all mobile devices--and not subject to phone manufacturer implementation quirks (_covered more in the next section_) when handling constraint settings like **exposureTime**, **whiteBalance**, **zoom** control, etc.
It turns out that, as Chrome on iOS uses `WKWebView` (and all in-app browsers follow that trend) and `WKWebView` is [not capable of using WebRTC](https://bugs.chromium.org/p/chromium/issues/detail?id=752458) to the degree we need, this is also an blocking situation for `webrtc-perception`.

To recap, on Android devices, advanced photography features are only partially implemented in the _MediaStream_ specification.
We are currently stuck with partial support on Safari on an Apple device.
Lastly, we have NO support on any other browser on an Apple device.

There is another more major issue to consider.

<hr>

### Inconsistent Implementations of Constraint Application on the Device Level

This is a _fun_ one, and I spent several days trying to determine how extensive this issue really is.
Once we found a consumer device that we could actually consistently work with, much of my focus was spent on working with that device.
Our measurements were largely taken with a [NVIDIA SHIELD K1](https://en.wikipedia.org/wiki/Shield_Tablet) tablet, which we have been using for the **`rtc-deflectometry`** application.
The few observations we made when initially evaluating the K1 leads me to believe that the behavior of certain constraints (_observations that are readily reproducible by tweaking with_ **whiteBalance** _settings on the K1_) will not be applied consistently across multiple devices as a developer would expect.

A picture of the controls with particular constrainable properties activated is shown below, and further down the page is the code that is executed when the remote device receives desired settings from the controlling client.

<div class="post-image">
    <a href="{{ site.url }}/{{ site.assets.posts }}/{{ page.specifics.images }}/03_constraint_settings.png">
        <img src="{{ site.url }}/{{ site.assets.posts }}/{{ page.specifics.images }}/03_constraint_settings.png" width="800">
    </a>
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

The state of each range bar and radio button is queried when hitting an **\[Apply Constraints\]** button further up the page (_not pictured_). Those are formatted in a way that _WebRTC_ can handle and transmit to the other device when the button is pressed.

The following sequence of behaviors is what occurs when I try to establish manual **whiteBalance** control and change the **colorTemperature** constraint setting, both of which should have a noticeable effect on our live camera feed.
The white balance should, if the constraint is being applied properly, track values displayed on the the [color temperature chart here on Wikipedia](https://en.wikipedia.org/wiki/Color_temperature).
Starting out from _automatic_, the camera [looks to be configured around 6000K]({{ site.url }}/{{ site.assets.posts }}/{{ page.specifics.images }}/04_wb_test_s0.png).
Note that the stated capabilities of the forward-facing camera on the **SHIELD K1** indicate that the color temperature can range from 2850K up to 7000K, so our test settings will all land somewhere between those extents.
Also note that these actions were taken exactly in this order, with the observed result at the end of each action:

1. Set **whiteBalance** to _manual_, set **colorTemperature** to _2850K_. **[Result]({{ site.url }}/{{ site.assets.posts }}/{{ page.specifics.images }}/05_wb_test_s1.png)** -> Displayed image shifts to displaying warmer tones, estimated to be somewhere around 3000K. 
2. Set **whiteBalance** to _automatic_, set **colorTemperature** to any random value (color temperature is not passed when setting an automatic **whiteBalance** mode in my code). **[Result]({{ site.url }}/{{ site.assets.posts }}/{{ page.specifics.images }}/06_wb_test_s2.png)** -> Camera converts back to automatic white balancing, which I estimate to be somewhere around 6000-7000K. This seems to be a different color temperature than what we started with, which is not unexpected behavior.
3. Set **whiteBalance** to _manual_, set **colorTemperature** to _7000K_. **[Result]({{ site.url }}/{{ site.assets.posts }}/{{ page.specifics.images }}/07_wb_test_s3.png)** -> No change.
4. Set **whiteBalance** to _automatic_, leave **colorTemperature** set to _7000K_. **[Result]({{ site.url }}/{{ site.assets.posts }}/{{ page.specifics.images }}/08_wb_test_s4.png)** -> The camera white balance is now even warmer than the results from the first step; I estimate around 2700K (possibly right at 2850K).
5. Set **whiteBalance** to _automatic_, set **colorTemperature** to _4000K_. **[Result]({{ site.url }}/{{ site.assets.posts }}/{{ page.specifics.images }}/09_wb_test_s5.png)** -> No change from prior step.
6. Set **whiteBalance** to _manual_, leave **colorTemperature** set to _4000K_. **[Result]({{ site.url }}/{{ site.assets.posts }}/{{ page.specifics.images }}/10_wb_test_s6.png)** -> No change from prior step, which is still estimated to be around 2700K.
7. Set **whiteBalance** to _automatic_, set **colorTemperature** back to _2850K_. **[Result]({{ site.url }}/{{ site.assets.posts }}/{{ page.specifics.images }}/11_wb_test_s7.png)** -> Camera seems to have automatically selected a color temperature somewhere around the 4000-5000K range.

As you can see, there is little rhyme or reason to the applied constraints and their effect on the video track; if anything, it seems like there might be a strange two-step approach to converting to manual control and then setting the desired color temperature.
I can confirm the constraints are being properly formed and handled by the device, as we always see a browser console warning log detailing any issues if we submit malformed constraints via the _MediaStream_ API.

In another example, on my **OnePlus 3T** Android phone, I noticed that the rear-facing camera allows the **torch** setting to be changed.
Enabling the torch does, in fact, turn on the rear LED when applying that constraint to the phone, but I cannot turn the LED off with the **torch** setting without locking the phone screen, which will automatically deactivate the LED until I go to apply the constraint again.
Attempting to set the boolean constraint for the torch to `false` does nothing, even though setting it to `true` and applying that constraint is what turns on the LED in the first place.
The **SHIELD K1** does not have a rear-facing LED to go with the rear-facing camera, so that setting is correctly unavailable and I cannot compare test results with the behavior of the K1 device.

After all that, it should be clearer as to why I am pretty frustrated with the _MediaStream_ APIs.
This is made all the worse as _MediaStream_ and _WebRTC_ together have incredible potential as a sophisticated camera and microphone control platform.
In terms of being able to facilitate all of the needs of `rtc-deflectometry` and `rtc-shapeshifter`, perhaps one day `webrtc-perception` can seamlessly support those applications across _ALL_ mobile devices, but this is not currently possible.
In the meantime, if you are have similar development aims and find yourself using _MediaStream_ and _WebRTC_, take these observations into account.
I sincerely hope you have more luck than I have had!
If your experience has been different with the _MediaStream_ API or you have other comments or suggestions, feel free to reach out via the Administrator contact info on the **[About]({{site.url}}/about)** page.
I would love to hear from you!
