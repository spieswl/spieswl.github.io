---
layout:             project
title:              "Adding a Digital Dashboard to My Mazda3"
date:               "2014-01-08"

description:        "Before most cars had Android Auto or Apple CarPlay, I added an tablet and OBD2 reader to my Mazda3 so I could have advanced navigation, vehicle diagnostics, and in-car intelligence at my fingertips."
keywords:           automotive, tablet, mobile devices, OBD2, DIY
tags:               [Mobile Devices, Automotive, DIY]

specifics:
    featured:       false
    images:         "adding-car-smarts"

published:         true
---

I really love it when I can breathe new life into something forgotten or neglected. This project evolved from a desire to recycle spare electronics left over from my undergraduate studies. Much of this tech no longer had the same level of usefulness it once had, but I was sure it could still be used in clever and interesting ways. One of my all-time favorite electronic devices is my old **[Asus Transformer TF101](https://www.laptopmag.com/reviews/tablets/asus-eee-pad-transformer-tf101)**, one of those convertible tablet/laptop hybrids that started emerging in the early 2010s. The **TF101** served me well for the last two years of my time in undergrad as a note-taking and browser-accessing compute device. It ran Android Honeycomb, which was the top-of-the-line Android release at the time. I had access to GPS, accelerometers, Bluetooth, and Wi-Fi. It was cheap, it was sturdy (_during one class, I dropped it concrete steps to audible gasps from horrified onlookers--who were amazed when it continued to work perfectly well afterwards_), and it was effective. After undergraduate schooling, I did not get any benefit from taking notes, accessing the web, and otherwise being productive on my **TF101**; those responsibilities had been picked up by my smartphone and my work-supplied laptop. Sadly, the **TF101** mostly collected dust from mid-2012 onwards...until I got this nifty idea on how to 'recycle' it.

My daily driver for my time in Columbus was a 4-door 2008 Mazda3; a beautiful vehicle to drive around in, although it was not exactly well-equipped with navigation or entertainment features. In fact, all it had was a 6-disc CD changer and a **_Line In_** port for hooking up a smartphone. Talking with some of my car-crazy colleagues at **ATS** about trying to upgrade my Mazda revealed that some of them had been toying around with Bluetooth OBD2 bus readers. Some further digging revealed that I could get an Android app called **[Torque](https://play.google.com/store/apps/details?id=org.prowl.torque&hl=en_US)** that could pair with Bluetooth-equipped OBD2 adapters and pull diagnostic data off a vehicle's CAN bus. All of a sudden, there was a path to 'upcycling' my old TF101 into a dash-mounted vehicle nav, infotainment, and diagnostics system! I just needed to find a decent OBD2 Bluetooth adapter (_I ended up buying a version of [this ELM327 adapter from NewEgg](https://www.newegg.com/p/0KZ-00HS-00017)_), maybe a wired charging cable for the TF101, and a dash mount to hold the tablet in place!

After some modifications to my Mazda's dash, plugging in the ELM327 adapter, and setting up Torque Pro with a nice looking gauge layout, I was now in business.

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

The Torque app and the Transformer **TF101** were an excellent combo. The app's flexible gauge layout provided an incredible level of information about my Mazda's running state. I could get vehicle error codes when needed. I could track other important running data like manifold pressure, coolant temperatures, air-fuel-mix ratios, etc. The fact that my **TF101** also had a GPS transciever allowed Torque to access Google Maps via a plugin, track my route, and overlay vehicle telemetry on every part of that route. I was also able to get turn-by-turn navigation through Google Maps, either through offline saved maps or by tethering the tablet to my smartphone. I could also load podcasts and music onto my new dashboard; a capability I only sometimes used thanks to the weird placement of the **_Line In_** jack inside the center console.

<div class="project-image">
    <a href="{{ site.url }}/{{ site.assets.projects }}/{{ page.specifics.images }}/05_mounting.png">
        <img src="{{ site.url }}/{{ site.assets.projects }}/{{ page.specifics.images }}/05_mounting.png" width="480" style="margin:16px 4px 16px 4px">
    </a>
</div>

The physical mount was crucial for keeping things in place. I previously mentioned the tablet was hefty; I resorted to buying a marine-grade tablet mount that I screwed into the dash. That area behind the hazard flashers was open except for the air vent conduits. The jaws holding the tablet had a heavy-duty spring keeping them closed, so the tablet was never in danger of falling out. About the only thing I had to give up was being able to look at the small multi-segment display behind the tablet, which only showed the time, trip mileage, and the outside temperature. In the event I could not take the tablet with me, most of the time I just put it in the glove compartment.

The payoff for this mini-project was huge compared to the miniscule time and financial investment. The fact that I was able to simultaneously reuse some old electronics and give my daily driver a major upgrade in smarts gave me a huge sense of pride. A few friends commented on how cool and useful that kind of integrated system felt whenever we took a drive. Lastly, it just felt really professional; I would have preferred not to drill into my dash, of course, but the tablet and mount were solidly fixated and had a great final look.
