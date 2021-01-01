---
layout:             post
title:              "Seven Years of Factorio Friday Facts"

description:        "This entry focuses on Wube Software's long-running Factorio Friday Facts; a seven-year labor of love documenting the creation of the titular video game."
keywords:           factorio, retrospective
tags:               [Factorio, Retrospectives]

specifics:
    images:         "2020/factorio-friday-facts"

published:          true
---

Few things are more enjoyable to me than reading about the experiences of a professional. There is just something special about getting a peek behind the veil of any serious endeavor, especially when the author's passion for the craft and for sharing that passion with others shines through. The field of science has many instances of this: the outreach work of figures like Carl Sagan and legendary talks like [Randy Pausch's last lecture](https://youtu.be/ji5_MqicxSo) are but a few examples of that elusive connection between science and engineering, and humanity. It is rare to find people that can take a technical subject, distill it into an easily digestible form, and then make the reader feel good in consuming that content. It is truly special to do all that and, at the same time, create a bond with fellow people.

**Factorio Friday Facts** (FFF) is a blog series started in 2013 by a duo of Czech software engineers. They initially intended to document their experiences while developing the computer game ***[Factorio](https://factorio.com/)*** as a way to keep in touch with their supporting community. While it started as the modest musings of two young professionals, over time their team [grew](https://factorio.com/game/about) and their game [became popular](https://steamdb.info/stats/gameratings/) (*ranked #2 in the world among **ALL** Steam games in user feedback, with over 2 million copies sold*). The simple end-of-week tradition where they talk about what's happening in their office turned into one of the best technical blogs around. The subjects of this series range from designing systems around user experience, graphics and GUI development, AI, network troubleshooting, memory access optimizations, a whole host of computer science topics, the process of creating art and sounds, the process of growing a team, the process of working through adversity, and the long, risky journey of building something truly new.

My wife knows well of my occasional addictions to ***Factorio***; it is one of the purest problem-solving, creation-oriented, whole-brain-tapping passtimes I can think of. Naturally, I lose myself in a world where I can freely create, refactor, optimize, explore, and experiment...and I am far from the only person who has done so. The developers behind this game are, in my opinion, some of the best in the business. Not only for making such a wonderful thing, but the dedication, effort, and thoughtfulness they display _even when engaging in the most menial of maintenance tasks_ is the stuff of legend. They have been wildly successful, maybe in spite of ***Factorio*** not being alluring to all, and in doing so have something remarkable into our world.

Today is the day they officially released their product to everyone. As a way to celebrate their incredible journey, I wanted to share some of my favorite entries from the last seven years of **Factorio Friday Facts**:

##### Getting Started
* **[FFF #1](https://factorio.com/blog/post/fff-1)**
* **[FFF #9](https://factorio.com/blog/post/fff-9)**
* **[FFF #14](https://factorio.com/blog/post/fff-14)**
* **[FFF #23 - Year After](https://factorio.com/blog/post/fff-23)**
<hr>

##### Graphics and GUI Design
* **[FFF #179 - New resource graphics & concrete](https://factorio.com/blog/post/fff-179)** : Cool description of using Python scripting to procedurally randomize resource tiles.
* **[FFF #191 - Gui improvements](https://factorio.com/blog/post/fff-191)**
* **[FFF #194 - Automated combinator pipeline](https://factorio.com/blog/post/fff-194)** : A fantastic look at a highly automated graphics generation pipeline.
* **[FFF #212 - The GUI update (Part 1)](https://factorio.com/blog/post/fff-212)** : Some of my favorite FFF entries are when they talk about GUI design and trying to understand the complex interactions of ***Factorio***.
* **[FFF #216 - Paving a path for the GUI update](https://factorio.com/blog/post/fff-216)** : A journey through their interface design over the years.
* **[FFF #238 - The GUI update (Part 2)](https://factorio.com/blog/post/fff-238)** : More user interface design.
* **[FFF #243 - New GUI tileset](https://factorio.com/blog/post/fff-243)** : Showing off their GUI design document. A **MUST READ**, if only for a good example of a comprehensive design style that fits the game theme perfectly.
* **[FFF #246 - The GUI update (Part 3)](https://factorio.com/blog/post/fff-246)** : Even something as simple as button layout is important.
* **[FFF #277 - GUI progress update](https://factorio.com/blog/post/fff-277)**
* **[FFF #279 - Train GUI & Modern Spitter](https://factorio.com/blog/post/fff-279)** : An interface as intuitive as the Train control GUI is rare...
* **[FFF #280 - Visual Feedback is the king](https://factorio.com/blog/post/fff-280)** : ...especially with the care taken to provide the level of feedback illustrated here.
* **[FFF #337 - Statistics GUI and Mod Debugger](https://factorio.com/blog/post/fff-337)** : The presentation of data is so important to the developers at Wube. See how they track and display factory performance statistics here.
* **[FFF #348 - The final GUI update](https://factorio.com/blog/post/fff-348)**
<hr>

##### An Incredible Culture of Optimization
* **[FFF #115 - The power switch](https://factorio.com/blog/post/fff-115)** : Good write-up here on graph optimization as it relates to some of their system designs.
* **[FFF #151 - The plans for 0.14](https://factorio.com/blog/post/fff-151)** : Optimizing their simulation update loop to work around a multithreaded architecture.
* **[FFF #176 - Belts optimization for 0.15](https://factorio.com/blog/post/fff-176)** : Incredible analysis of transport belts and optimizing their performance. A **MUST READ**.
* **[FFF #182 - Optimizations, always more optimizations](https://factorio.com/blog/post/fff-182)**
* **[FFF #201 - 0.15 Stable, but not really](https://factorio.com/blog/post/fff-201)** : *"There's always something else that can be optimized."*
* **[FFF #204 - Another day, another optimisation](https://factorio.com/blog/post/fff-204)** : Solid technical writeup on how a prefetching optimization patch works across a number of resource-heavy factory builds. A **MUST READ**.
* **[FFF #206 - Workflow optimisation](https://factorio.com/blog/post/fff-206)** : Speeding up the change / compile / test loop.
* **[FFF #209 - Optimisation is a way of life](https://factorio.com/blog/post/fff-209)**
* **[FFF #260 - New fluid system](https://factorio.com/blog/post/fff-260)** : Modeling fluids is hard, extremely hard.
* **[FFF #264 - Texture streaming](https://factorio.com/blog/post/fff-264)**
* **[FFF #271 - Fluid optimisations & GUI Style inspector](https://factorio.com/blog/post/fff-271)** : Another day, another fluid system optimization.
* **[FFF #317 - New pathfinding algorithm](https://factorio.com/blog/post/fff-317)** : A FFF relevant to robotics! Pathfinding at an insane scale using A* and clever chunk reduction to facilitate pathfinding.
* **[FFF #331 - 0.18.0 release & Train pathfinder changes](https://factorio.com/blog/post/fff-331)** : Continuing optimizations involving train pathfinding.
<hr>

##### Crazy Times in Troubleshooting Networking
* **[FFF #136 - Map Transfers](https://factorio.com/blog/post/fff-136)** : As multiplayer functionality was added, more FFF segments start focusing on networking...
* **[FFF #143 - Matching server and UDP NAT punching](https://factorio.com/blog/post/fff-143)** : Interesting to see `STUN` and `ICE` mentioned here in 2016; I will later spend time studying the same things when working on `webrtc-perception`.
* **[FFF #196 - Back on track](https://factorio.com/blog/post/fff-196)** : They create their own Wireshark plugin in order to do deep packet introspection.
* **[FFF #302 - The multiplayer megapacket](https://factorio.com/blog/post/fff-302)** : Experiments with hundreds of connected players in a multiplayer session leads to a discovery that a connection recovery mechanism is not behaving as expected.
<hr>

##### On Software Engineering
* **[FFF #46 - Knowledge sharing](https://factorio.com/blog/post/fff-46)**
* **[FFF #55 - MP preview](https://factorio.com/blog/post/fff-55)** : This has a great section on determinism testing, which is hugely important for a game so reliant on maintaining a deterministic simulation.
* **[FFF #60 - Tests all around](https://factorio.com/blog/post/fff-60)** : Some miscellaneous information bookends a succinctly written understanding of how each abstract level of automated testing improves Wube Software's ability to maintain performance.
* **[FFF #112 - Better noise](https://factorio.com/blog/post/fff-112)**
* **[FFF #215 - Multithreading issues](https://factorio.com/blog/post/fff-215)** : Troubleshooting a multithreading issue...
* **[FFF #265 - Nomenclature & Steam networking](https://factorio.com/blog/post/fff-265)** : Nomenclature is important. Uncertainty in what something means or how something is referenced can lead to other problems in development.
* **[FFF #274 - New fluid system 2](https://factorio.com/blog/post/fff-274)** : Not often do you get the help of a physicist from your community to implement a fluid simulation model.
* **[FFF #296 - All kinds of bugs](https://factorio.com/blog/post/fff-296)** : The snippet at the bottom is a perfect encapsulation of their attitude towards problem solving. It's such a simple concept, just going a little farther, that can take tremendous discipline and focus. A **MUST READ**.
* **[FFF #312 - Fluid mixing saga & Landfill terrain](https://factorio.com/blog/post/fff-312)** : Before reading this, go check out how much work has been put into fluid simulation...then read this one about breaking it.
* **[FFF #349 - The 1.0 plan](https://factorio.com/blog/post/fff-349)** : The release date moves closer. They show off more debug and introspection capabilities, including a fascinating prototype inspection system.
<hr>

##### The Journey
* **[FFF #34 - Sales, Support, Stress and Steam](https://factorio.com/blog/post/fff-34)**
* **[FFF #81 - Chain signals](https://factorio.com/blog/post/fff-81)** : Some incredible effort / results comparisons at the beginning of this article. We will come back to these statistics later...
* **[FFF #102 - Getting close](https://factorio.com/blog/post/fff-102)** : A few anecdotes about database management give way to observations on success after seeing how it affected one of their most well-known contemporaries.
* **[FFF #135 - Getting Organized](https://factorio.com/blog/post/fff-135)** : Growth leading to higher organization and complexity. A peek at how this starts to affect Wube Software.
* **[FFF #184 - Five years of Factorio](https://factorio.com/blog/post/fff-184)** : Oh yeah, they also talk about optimizations here too. These people are relentless.
* **[FFF #192 - One million](https://factorio.com/blog/post/fff-192)** : Five years after the first commit, they reach one million sold copies.
* **[FFF #200 - Plans for 0.16](https://factorio.com/blog/post/fff-200)** : ***200*** fridays in a row with no misses. Incredible!
* **[FFF #223 - Reflections on 2017](https://factorio.com/blog/post/fff-223)** : Revisiting the statistics from **FFF #81**, this time after 5 years and 9 months of continuous development.
* **[FFF #269 - Roadmap update & Transport belt perspective](https://factorio.com/blog/post/fff-269)** : A quick update at the beginning talks about some of the realities of complex software development sticking to a deadline.
* **[FFF #294 - Blog thoughts & Lua documentation improvements](https://factorio.com/blog/post/fff-294)** : A look at the pageview metrics for FFF, and some thoughts about what this weekly routine means to them.
* **[FFF #309 - Controversial opinions](https://factorio.com/blog/post/fff-309)** : It should surprise no one with experience that the developers have some "controversial" opinions about their own creation.
* **[FFF #327 - 2020 Vision](https://factorio.com/blog/post/fff-327)** : Nearing the end of the pre-release development period. It's been a long time coming...
* **[FFF #352 - New website](https://factorio.com/blog/post/fff-352)** : While this could probably be in the graphics / GUI section, this is really about how their project (and everything that it has touched) has matured over the past several years.
<hr>

##### The Human Experience
* **[FFF #69 - Sympathy for the creeper](https://factorio.com/blog/post/fff-69)** : A clever twist on the perspective of the player-character as given by the game's art director.
* **[FFF #224 - Bots versus belts](https://factorio.com/blog/post/fff-224)** : A philosophical discussion about how some components in the game overincentivize a certain kind of playstyle, much to some developers' chagrin.
* **[FFF #225 - Bots versus belts (part 2)](https://factorio.com/blog/post/fff-225)** : A controversial **FFF #224** leads to some community feedback that the developers have to deal with.
* **[FFF #356 - Blueprint library for real](https://factorio.com/blog/post/fff-356)** : Michal "kovarex" Kovařík, one of the two cofounders, talks about falling out of the development process, losing his sense of purpose, and how playing ***Factorio*** with his son brought him back. Reading this one, even through the sometimes-broken English, is inspirational and a wonderful look at how things fall apart and come back together. Like I mentioned in the intro, this is one of those rare, special offerings that is technical in nature but deeply personal and human-centric in form. A **MUST READ**.

...and, finally,

* **[FFF #360 - 1.0 is here!](https://factorio.com/blog/post/fff-360)** : All good things must come to an end. The developers elect to end the blog series on the same day they celebrate the long-awaited 1.0 release.

<hr>

It is hard to think of the right things to say at the end of all this. I believe there are many endeavors that can reveal the nature of the universe IF you have the will to see it; inspiration can come from anywhere. When I consider that belief, it makes me proud to share the work of these humble video game creators. For the past several years, I have been reading their posts, playing their game, and following their journey. I consider myself incredibly lucky to have been watching them navigate the trials and tribulations of creating something new and unique, but a profound sense of sadness accompanies the feelings of joy. Attending the release of the final *Lord of the Rings* movie, watching the last launch of the Space Shuttle, moving away from my hometown to chase new opportunities, losing track of old friends; all of these have the same bittersweet taste as reading the ***Factorio*** team's final post. The connections I have with all of those things are specific and personal, of course, but I hope to be able to relay just a fraction of what this content means to me as an engineer, as a creator, as a person. If they do not pique your interest, that is okay too. Instead consider that the world has many great and inspirational things to offer, many of which you may have overlooked or could be hidden in plain sight. Look harder for those things and you will be better off as as person.

And yet, even if you never read any of the above, we can still to sit down and build something in ***Factorio*** together ... and that is a wonderful thing.
