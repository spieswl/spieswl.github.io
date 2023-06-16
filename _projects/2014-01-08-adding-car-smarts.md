---
layout:             project
title:              "Adding a Digital Dashboard to My Mazda3"
date:               "2014-01-08"

description:        "Before most cars had Android Auto or Apple CarPlay, I added an tablet and OBD2 reader to my Mazda3 so I could have advanced navigation, vehicle diagnostics, and in-car computation at my fingertips."
keywords:           automotive, tablet, mobile devices, OBD2, DIY
tags:               [Mobile Devices, Automotive, DIY]

specifics:
    featured:       true
    images:         "adding-car-smarts"

published:         true
---

One of my pet peeves is when I have some electronic device that is useful for a period of time but eventually loses utility as the battery dies or product support ends. Sadly, the ultimate end for these electronics is usually at an electronics recycler or in a long-forgotten box in the closet. This project is about a successful reuse of some older technology to do something novel with my car. One of my all-time favorite computers is an **[Asus Transformer TF101](https://www.laptopmag.com/reviews/tablets/asus-eee-pad-transformer-tf101)**, one of those convertible tablet/laptop hybrids that started emerging in the early 2010s. The **TF101** served me well for the last few years of my undergraduate career as a note-taking and browser-accessing computer. It ran Android Honeycomb, which was the top-of-the-line Android release at the time. I had access to GPS, accelerometers, Bluetooth, and Wi-Fi. It was cheap, it was sturdy (_during one class, I dropped it concrete steps to audible gasps from horrified onlookers--who were amazed when I picked it up like nothing happened and it continued to function perfectly well afterwards_), and it was good at its job. After undergraduate schooling finished, its use as a college computer and otherwise niche productivity tool was heavily diminished. Those responsibilities had been picked up by my smartphone and my work-supplied laptop. The **TF101** mostly collected dust from mid-2012 onwards...until I got this nifty idea on how to 'recycle' it.

My daily driver at the time was a 4-door 2008 Mazda3; a fun vehicle to drive around in, although it was not exactly top-of-the-line when it came to navigation or entertainment features. In fact, all it had was a 6-disc CD changer and a 3.5mm **_Line In_** port for hooking up a smartphone. While talking with some of my car-crazy colleagues at **ATS** about tinkering, I discovered that some of them had been toying around with Bluetooth OBD2 CAN bus readers. Some investigation on my own revealed that I could download an Android app named **[Torque](https://play.google.com/store/apps/details?id=org.prowl.torque&hl=en_US)** which could pair with Bluetooth-equipped OBD2 adapters and pull diagnostic data off a car's CAN bus. All of a sudden, there was a path to 'upcycling' my old TF101 into a dash-mounted vehicle nav, infotainment, and diagnostics computer! I just needed to find a decent OBD2 Bluetooth adapter (_I ended up buying a version of [this ELM327 adapter from NewEgg](https://www.newegg.com/p/0KZ-00HS-00017)_), a wired charging cable for the TF101, and a dash mount to hold the tablet in place.

After some mechanical modifications to my Mazda's dash, hooking up the ELM327 adapter, and setting up Torque Pro with a nice looking gauge layout, I was now in business.

<div class="project-image">
    <a href="{{ site.url }}/{{ site.assets.projects }}/{{ page.specifics.images }}/01_driver_view.png">
        <img src="{{ site.url }}/{{ site.assets.projects }}/{{ page.specifics.images }}/01_driver_view.png" width="480" style="margin:4px 4px 4px 4px">
    </a>
    <a href="{{ site.url }}/{{ site.assets.projects }}/{{ page.specifics.images }}/02_dash_view.png">
        <img src="{{ site.url }}/{{ site.assets.projects }}/{{ page.specifics.images }}/02_dash_view.png" width="270" style="margin:4px 4px 4px 4px">
    </a>
</div>

<div class="project-image">
    <a href="{{ site.url }}/{{ site.assets.projects }}/{{ page.specifics.images }}/03_gauge_display.png">
        <img src="{{ site.url }}/{{ site.assets.projects }}/{{ page.specifics.images }}/03_gauge_display.png" width="480" style="margin:16px 4px 16px 4px">
    </a>
    <a href="{{ site.url }}/{{ site.assets.projects }}/{{ page.specifics.images }}/04_map_display.png">
        <img src="{{ site.url }}/{{ site.assets.projects }}/{{ page.specifics.images }}/04_map_display.png" width="480" style="margin:16px 4px 16px 4px">
    </a>
</div>

The Torque app and the Transformer **TF101** were an excellent combo. The app's flexible gauge layout provided an incredible level of information about my car's running state. I could get vehicle error codes when needed. I could track other important running data like manifold pressure, coolant temperatures, air-fuel-mix ratios, etc. The fact that my **TF101** also had a GPS reciever allowed Torque to access Google Maps via a plugin, track my route, and overlay vehicle telemetry data on that route. I was also able to get turn-by-turn navigation through Google Maps, either through offline saved maps or by tethering the tablet to my smartphone. I could also load podcasts and music onto my new infotainment tablet; although that capability I only sometimes used thanks to the weird placement of the **_Line In_** jack inside the center console.

<div class="project-image">
    <a href="{{ site.url }}/{{ site.assets.projects }}/{{ page.specifics.images }}/05_mounting.png">
        <img src="{{ site.url }}/{{ site.assets.projects }}/{{ page.specifics.images }}/05_mounting.png" width="480" style="margin:16px 4px 16px 4px">
    </a>
</div>

The physical mount was crucial for keeping things in place. I previously mentioned the tablet was hefty; I resorted to buying a marine-grade tablet mount that was screwed into the dash. The area behind the hazard flashers was open except for the air vent conduits, so there was ample space to affix the mount base. The jaws holding the tablet had a heavy-duty spring keeping them closed, so the tablet was never in danger of falling out. About the only thing I had to give up was being able to look at the small multi-segment display behind the tablet, which only showed the time, trip mileage, and outside temperature. In the event I could not take the tablet with me, it could be locked away in the glove compartment.

Overall, this was a huge upgrade for a meager financial investment, some elbow grease, and the salvaging of an old computer. The fact that I was able to simultaneously reuse my old electronics and give my daily driver a major intelligence upgrade made this a hugely satisfying use of a weekend. A few friends commented on how cool and useful that kind of integrated system was whenever we went on a drive. Lastly, it looked far more professional than it had any business being; I would have preferred not to drill into my dashboard, of course, but the tablet and mount were solidly affixed and had a great final look. At a time where only premium sedans and luxury vehicles were getting the kinds of infotainment capabilities the TF101 could provide, this ended up being a far easier and far cheaper way to get the same result.
