---
layout:             feature
title:              "ATS Automation"
date:               "2018-08-04"

description:        "My memories of an incredible time spent in the field of manufacturing automation and industrial robotics."
keywords:           memoirs, automation, robotics, manufacturing, engineering
tags:               [Memoirs, Robotics]

specifics:
    career:         true
    project:        false
    images:         "ats-automation"

published:          false
deadhead:           false
---

I never imagined I would be working in manufacturing when I entered my Senior year of undergraduate studies.
With a degree in electrical engineering, and what seemed like my entire life ahead of me, my outlook was good.
18 months of working a co-op job in the space systems department of a defense contractor had been a stable and serious first experience as an engineer.
I had always wanted to work on things that were destined to go into outer space, and having already been able to do that at 23, even in the limited ways a co-op contributes, was a dream-come-true.
Alas, my employer had no room for me (and many other young engineers) when a sister division was forced to shut down due to broader economic and defense procurement trends in 2012, and the displaced senior were given priority for filling new positions.
This was a prudent and considerate thing for the parent corporation to do, considering the situation, but I was among many others who were now forced to look elsewhere for my first engineering job out of school.

My forray into manufacturing, and what turned out to be a life-changing series of opportunities, was at ATS Automation up in Columbus, Ohio.
I was hired on as an electrical design engineer in June of 2012 and immediately brought down the average age of the electrical department by about 10 years.
By the time I had left ATS to attend graduate school at Northwestern in 2017, I had personally worked on 12 assembly automation systems, was involved in designing a couple others, and had worked on everything from toothbrush head fusing to solar panel fabrication to electric vehicle battery assembly systems.
Along the way, I worked alongside and made friends with some exceptional people.
I was able to work on industrial robotics at a scale that many people still find hard to fathom.
I was part of a team that provided automation solutions for corporations like General Motors, First Solar, Tesla Motors, Dow Chemical, TRW, Oral-B, Doosan, and others.

WHen I joined, I became one of the youngest employees in my division--not just engineers, but among all employees.
This fact had implications for the division I worked in, our international parent corporation as a whole, and the manufacturing industry in its entirety.
A race to get younger, more technologically integrated, and more robotic was seemingly in process across the manufacturing industry, as general robotics and industrial automation started blending in academic and research circles not long before I started at ATS.
I, and ATS Columbus, saw this trend first-hand this over the many projects that matured in that time.
In fact, this trend was the exact reason I had sought out graduate studies in robotics, but we can talk about that later.

What follows is a brief synopsis of what I worked on from 2012 to 2017.
The first sections are a technical rundown, sanitized, where necessary, of company proprietary information.
The second sections are tailored towards my memories of my interactions with colleagues and of the project experiences themselves.
At some point, the arrangement of the two sections will flip as the technical content loses its importance.

### Technical Overview

#### Dow PowerHouse Solar Shingle Final Test and Sort (ca. mid 2012)

Mostly, I was working on or interacting with...

* Updating and adding new safety-rated hardware (**Allen-Bradley** safety relays and sensors), documentation and schematic updates, and procurement
* Presenting changes to _Dow_ engineers during document reviews

#### Dow PowerHouse Solar Shingle Hipot Testing (ca. late 2012)

I worked with a lot of custom electonics and wiring systems on this project, but some off-the-shelf components were notable, such as...

* **Fortress** safety interlock systems,
* **Vitrek** "hi-pot" testers capable of generating high voltage (>5000 V),
* **Allen-Bradley (AB)**  multi-phase contactors for exposing high voltage to the panels

...while also helping to perform the final system run-off before getting _Dow_ acceptance to ship.

#### TRW Iso/Dump Safety Valve Fabrication (ca. 2013)

I was tapped to work on some updates and modernizations for this system.
There was a lot of work in designs that had been produced by other engineers, so my reverse engineering chops were tested.
As this was a design of European origin, a lot of European hardware was present:

* **Siemens** PLCs and communications backbones, specifically _Profibus_ (and later _Profinet_ when the upgrades completed),
* **Pilz** safety relays and sensors,
* and a pretty exotic conveyance system called **SuperTrak**, which operates on a magnetic conveyance principle.

The first generation designs were eventually modernized and a new, second-generation system was commissioned for _TRW_ to operate at their facility in tandem with the first.

#### Exotic Metal Press Concept (ca. 2013)

Unfortunately, I can't go into the details on this project.
However, I did spent a lot of time working side-by-side with our applications department what to procure and integrate for the concept.

#### General Motors Air-Cooled Battery Stacker and Module Final Assembly (MFA) (ca. 2013 to 2015)

My first project acting as lead was also my first experience putting a lot of concepts and technologies together, like...

* **Fanuc** robotic arms, such as the **R-2000iB** and **M-3iA**,
* **AB** safety PLCs, servomotor systems, and safety I/O modules,
* **Siemens** HMIs (_this was also around the time that_ **Stuxnet** _was revealed to have been impacting Siemens automation systems, so we had to pay attention to software and firmware revision numbers when purchasing hardware for GM_), RFID readers, and RFID tags,
* **Banner** lighting and operator signaling hardware,
* **Omron** safety light curtains,
* **SMC** pneumatics,
* **Turck** field networked I/O modules (which used ***DeviceNet***, a.k.a. ***CIP*** over CAN bus),
* all kinds of **Bosch** conveyance and motor hardware,
* **Keyence** vision systems,
* high-powered laser welding systems,
* electrical testing systems, and
* one hand-off interaction to a **Daifuku-Webb** AGV system.

This was heavily facilitated by _GM_'s tight integration with **ePlan**, which allowed for a lot of assistance in the form of automated reporting, parts aggregation for BOM generation, project-level version control, automatic schematic generation for certain things, etc.
One example of this was that I could do some back-of-the-napkin calculations of what sized power distribution panel (PDP) I needed to be able to support 2 robotic arms, several field devices, and other equipment.ly safety systems) for an end-of-line "solar simulator" and tester, adhesive dispense station (tar for the back of the shingles), and r
Just by adding that "component" (it was really a fully-featured panel equipped to _GM_ specifications) to the project, the CAD software added additional schematic diagrams with open sockets on the drawings for all of the connections to that panel.
From there, it was simply a matter of organizing what material needed to be connected where, ensuring your calculations were correct, and pressing the button to update the Bill of Materials.

The overall system was a little bigger than a school gymnasium, and the whole thing was impressive to see in operation when it came time for field acceptance testing and, later, site acceptance testing at the designated _GM_ facility.

#### General Motors Gen. 1 Volt Battery Assembly Upgrades (ca. 2014)

The primary area of interest here was in retrofitting several robotic welding cells on the **Volt** battery assembly systems with newer versions of the ultrasonic welders they were using.
Beyond that, there wasn't anything particularly interesting about working on that project.

#### Dow FILMTEC Water Filter Fabrication (ca. late 2014)

I started a transition over to PLC programming on this project.
Management was eager to have someone with my electrical skillset and modern coding experience work on the actual PLC programming aspect, so I was less involved in the system design and more involved with the system programming, which entailed...

* **IEC 61131**-style programming, specifically the ladder logic layout, on an **AB ControlLogix** platform,

This really boiled down to commissioning the system, updating tags and addresses, fault messages for HMI display, adding some software to control new features that had been added in hardware, and running the machine for customer acceptance until it shipped.

<hr>

### What It Was Like

#### Starting Out with Dow PowerHouse

My first set of tasks was to provide support on a _Dow PowerHouse_ project that had been well-underway by the time I had joined ATS.
I was exceedingly junior, and my first work was to cut my teeth on updating electrical schematics (specificalobotic sortation system.
The robot (a **Fanuc R2000**) was on a 7th axis and was responsible for sorting the panels after getting the test results.
When it had properly sorted the finalized panels, the robot would load the dunnage holding those panels into large garage bays for later forklifts to remove at the Dow facility this machine would inhabit.
It was a pretty big project to work on as a first go around, but I wasn't yet contributing much.

A later project I worked on that year was further upstream in the overall solar panel assembly system: an automated hipot test system that would take a rack of solar panels and dunk them in water before applying high voltage to the electronics.
Hipot (or high potential) testing is critical for ensuring that the electronics in a solar panel and its adjacent PV systems are adequately insulated from the outside world, especially for those cases where rain and snow might cause problems later.
I worked a lot with one engineer in particular who would later become a close friend and mentor, _Ralph Kerr_, on the electrical and PLC programming side of things.

I was largely making sure Ralph had whatever hardware he needed as the hipot testing requirements were particularly sensitive; so much effort was spent in studying any and every element of the overall machine to eliminate outside EMF and other sources of measurement error.
All in all, this was a pretty wild project; dunking solar panels in water, detecting utterly tiny amounts of leakage current, spraying things down while testing was occuring, all kinds of unique and exotic things were done in the name of exercisi
ng these panels.
I didn't directly contribute to the design, but I got to see how environmental testing (or, at least, one way of doing it) occurred in the factory environment.

#### Moving onto TRW Projects

While the Dow projects were wrapping up, I was shifted to some design upgrades that involved assembly automation systems that _ATS_ overall had provided to _TRW_.
These machines were much different in size, technology used, and the scale of the thing being assembled when compared to _Dow_.
There were three particularly interesting aspects to that system:

1. The part being produced (which was important to safety systems that _TRW_ sold) was scarcely larger than a quarter,
2. There was a unique magnetic conveyance system that conveyed all the pieces throughout the entire machine, and
3. The system had been designed by a sister division of ours in Germany.

Particularly due to that last point, I ended up getting very familiar with **IEC** standards for schematics (as opposed to the **NFPA** style that North American companies preferred) and a lot of Germany-sourced hardware.

This would later lead to future projects that would bring forth a second generation system based on designs I updated from the first.
I spent a small amount of time on the road in Fenton, Michigan while working on both the first and second generation systems, during some fairly dark winter months.
Between being on the road and the dark Michigan winter, it was challenging for me, to say nothing about how that must've been for the programmers and assembly crew who were there far longer.
It paid off, though.
The second generation system eventually surpassed the first in terms of uptime and quality, which was a particular source of pride for our division.

#### Exotic Metal Press Concept (Details NDA/Proprietary)

Sometime between the TRW job and the much larger GM project (which is next up), I was asked to provide some designs for an experimental concept our applications department had been working on with a research lab.
Details for all of this are trimmed to a great degree, including the patents, concepts, and involved parties, as part of my professional responsibilities here, but I can talk in some broad generalities.
It was a pretty exotic concept that we had been engaged with to turn into a standardized process; something that could be part of a modern manufacturing environment instead of just a laboratory concept.
I was the only electrical engineer assigned to this, as the applications engineer _Rich Snodgrass_, was providing mechanical support, so I got to put together some great looking designs in **ePlan**.
The status of that project is secret, I have to say, but working on an entirely experimental concept was fascinating and really fun.

#### General Motors Air-Cooled Battery Systems

In late 2013, I was tapped to lead my first project.
We had won the project to provide a new assembly system to **General Motors** that would assemble an air-cooled battery pack.
The project manager, _Phil Taylor_, took a bit of a chance on me, having barely a year of experience under my belt, but his decision gave me the opportunity to take a huge role in this and future projects.
As the lead electrical engineer, I put together the designs for two individual "systems" that were meant to connect as part of a larger operation.
One system, the _Stacker_, was taking many of the individual pieces and assembling them into a larger _Module_, albeit with assembly tasks still not yet performed.
The other system, _Module Final Assembly_, would take the "stacked" battery and perform the last assembly tasks, like laser welding, electrical testing, inspection, and pass it on to other parts of the warehouse (specifically, an AGV system that GM operated in their battery assembly plant that would convey the final module to other stations beyond).

Part of this involved spending time being trained at GM facilities in Warren, Michigan on their global controls standards, their design tools, and other things important to providing standardized schematics and project deliverables.
I came away with a great appreciation for how much work that GM had taken care of for the system designers, and although one of my programming colleagues, _Chris Lannoye_, had to deal with much more software as a result, the finished system was considered one of the best we had provided to General Motors.

This project took almost two years, but along the way, I worked with a massive amount of automation hardware.
Part of _GM_'s requirements were to commission the project designs in a tool named **ePlan**, which I would later help to deploy across ATS Columbus as a whole to great effect.
The electrical department was able to produce higher quality and more extensive drawings of their work with **ePlan**, particularly through the use of plugins that help with procurement and Bill of Materials generation, listing out the auxiliary labels needed for wiring, tagging, etc, and in the commissioning of system documentation that was usually required as part of the deliverables provided to customers.
The GM air-cooled battery packs produced by the machines eventually went into the _Chevrolet Silverado_ and _GMC Sierra_ platforms, and the machines ran so well that ATS later secured some pretty big follow-on projects with other GM electric vehicle implications.

### Circa 2014

#### Upgrades to the Gen. 1 Volt Battery Assembly Systems

As a prologue to later GM projects, I also was asked to design some upgrades to the ultrasonic welding systems that had been used to weld together portions of the battery packes used in the first generation **Chevolet Volt** plugin-hybrid-electric vehicles (PHEVs).
Not much is notable here, except for the fact that the welding technique used for the **Volt** batteries was different than that of the welding technique used on the eAssist battery packs.
I did get to spend some time up in Detroit to see the original Gen. 1 systems, which was pretty cool.

#### Dow FILMTEC Water Filter Fabrication System
