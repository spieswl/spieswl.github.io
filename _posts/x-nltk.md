---
layout:             post
title:              "Using Python and the Natural Language Toolkit on Windows with Cygwin"

description:        "For those looking to have the best of both the Windows and Linux worlds, Cygwin can bridge the gap. While this is not installing Python on Windows through the traditional means, we instead gain some of the benefits of using Linux. Here we install Python and some support libraries, specifically the Natural Language Toolkit."
keywords:           python, cygwin, NLTK, windows, pip
tags:               [Cygwin, NLTK, Python]

specifics:
    images:           "nltk"

published:          false
---

Python is POPULAR. HackerRank recently released their [2018 Developer Skills report](https://research.hackerrank.com/developer-skills/2018/ "HackerRank 2018 Developer Skills Report") and, in my opinion, one of the least surprising results is that Python is the most loved programming language among the nearly 40,000 developers polled. It was also ranked second highest among languages that those polled would like to learn. None of the reasons given are all that surprising; excellent support for scientific and mathematic libraries coupled with perhaps the easiest syntax to follow means that Python can support the needs of budding programmers and complex applications with very few compromises.

It was unsurprising to me, then, when a friend of mine with a background in Classics asked me about setting Python up. He inquired about getting more serious with Python, specifically so he could use the [Natural Language Toolkit (NLTK)](http://www.nltk.org/). His professional and technical background is such that he could easily handle Linux, but he is not a person who would need an entirely new operating system for this type of work. Knowing that he might like to stay in Windows exclusively does add a slight degree of complexity...but even that is easily handled by something like [Cygwin](https://www.cygwin.com/). Below, I have transcribed the setup process I had him go through, for future reference.

_Special note for those with some understanding of Linux and/or Windows:_ Normally, I would just say "Install a Linux distribution and use Python there" or "Download and install Python and the NLTK to your Windows environment", but I opted for a hybridized approach. I knew my friend could handle the extra technical complexity (and might even enjoy the benefits) of a Linux-like environment. I also **really** did not want the answer to tack on the extra overhead of managing an additional OS, especially knowing how much he would end up staying in Windows. Plus, there is the added fun of trying something new! Tar and feather me as you will.

<hr>

### Installing Cygwin and Python 2.7 / 3.x

Since I have started using both Linux and Windows side-by-side, I have come to greatly appreciate working from a command line interface for development tasks. For the kind of Python development and package management that would be simple in Linux, Cygwin will make those tasks feel a little more natural in Windows without too much added hastle. For those wishing to add Cygwin to their Windows environment, follow these steps:


* 1) Navigate to the [Cygwin install page](https://cygwin.com/install.html).
* 2) Depending on your machine configuration, download either the **32-bit (setup-x86.exe)** or **64-bit (setup-x86_64.exe)** executables.
* 3) Start Cygwin setup (this will NOT install Cygwin until after you select packages for inclusion).
* 4) Set the "Download Source" at your discretion. This will typically be "Install from Internet".
* 5) **IMPORTANT**: Select a new "Root Install Directory", if desired.
* 6) Set the local package directory location at your discretion.
* 7) Set your connection method to the Internet.
* 8) Step through the "Download Sites" and "Progress" prompts to get to the "Select Package" screen.

There are thousands of packages here to choose from. Note that, for first time installation, a minimal subset of critical packages will be selected by default. Clicking on an entry's name will change the installer's behavior relative to that package, but you should keep the default packages that start as being selected at this moment.

For the moment, we care about adding Python and some key Python support libraries, such as **numpy**, to our list. You can search for specific packages using the Search window near the top of the screen. Be warned that even a specific query, such as one for "python3" will list dozens of packages. I **strongly** encourage reading the Package field for each entry when making selections. Unfortunately, the NLTK is not available here at this time, so we will have to install that in a separate step.

* 9) Search for and select `python3`, `python3-setuptools`, `python3-pip`, and `python3-numpy` at a minimum.
  * If you want the Python 2.7 versions instead of Python 3.6 (as of this writing) versions, replace `python3` with `python2` as necessary.

* 10) _OPTIONAL_: Search for and select `python3-ipython`.
  * I personally recommend installing **IPython**. It is an interactive shell for Python 2/3, and I like using it significantly more than the integrated Python interpreter for quick prototyping.

* 11) Click "Next" after selecting the desired packages. Required dependencies will then be automatically identified and detailed as part of the installation procedure. Click through to finish and select Desktop or Start Menu shortcuts as desired!

<hr>

### Loading up Cygwin, Starting Python, and Installing the Natural Language Toolkit

Cygwin operates by presenting the user with a terminal instance in Windows the same way a user might use the terminal on a Linux system. The difference is that Cygwin is operating out of an emulated environment contained in the installation location provided during Step 5 up above. If you navigate to that location on your Windows file system, you should see a `Cygwin.bat` file that, when executed, starts the Cygwin terminal. Alternatively, if you opted to let the installation create a shortcut on your desktop or in the Start menu, the supplied shortcut will point to this batch file.

We still, however, need to use [pip](https://pip.pypa.io/en/stable/) (one of the packages we grabbed in the last section) to download the NLTK. We can then include it as part of any other Python script we would run in our Cygwin environment.

* 1) In any case, launch Cygwin. You should be presented with a command line interface, such as this one below.

<img src="{{ site.url }}/{{ site.assets.posts }}/{{ page.specifics.images }}/P001.png" style="width:900px; padding:4px 4px 4px 4px;display: block">

* 2) At the command prompt, enter `python3 -m pip list`. That command will present a list of installed Python wheels (support libraries).
  * If using Python 2.7, instead enter `pip list`.

* 3) Install the NLTK by entering `python3 -m pip install -U nltk`.
  * If using Python 2.7, instead enter `pip install -U nltk`

* 4) Verify that the NLTK was installed by opening ipython (`ipython3`) OR the integrated Python interpreter (`python3`) and entering `import nltk`.
  * If no exceptions are raised, then you are good to go! Try typing in `nltk.` and hitting the Tab key to let "Auto-Complete" show the available list of NLTK methods.
  * You can further verify, such as if an exception kicks up immediately after the import command, by going back to the command prompt and listing installed Python wheels with `python3 -m pip list`.

At this point, the Natural Language Toolkit should be installed and ready for use. Adding the NLTK Data set is an optional, but recommended step.

-----
### Installing the [NLTK Data set](http://www.nltk.org/nltk_data/)

Thanks to the NLTK developers, a nicely integrated method for managing the NLTK dataset is available straight from the command prompt. When running an interpreter like **IPython** or **IDLE**, simply import the NLTK module and enter `nltk.download()`. A simple text interface will allow you to download selected components of the data set (or all of it at once) for use. However, I do not believe there is a way to remove pieces of the data set without going to the containing folder on your file system and deleting the relevant data from there.

Otherwise, you could simply enter `python3 -m nltk.downloader all` at the Cygwin prompt.

Other install support can be found [here](http://www.nltk.org/data.html).

<br>

#### Special Note on Cygwin and pip

I noticed, as I was going through the installation process myself _(I use Cygwin for compiling C/C++ code using GCC while in Windows instead of going through Visual Studio)_, that there is some confusion about pip's operational state in recent builds of Cygwin. Nearly all of the Google results, Stack Overflow questions, etc. about using Python and pip with Cygwin indicated that pip would not work out-of-the-box, so to speak, after Cygwin completed installation; presumably because pip had not been made available as a package downloadable from repositories that Cygwin targets when performing the install step. 

I found those issues to be **resolved**, at least as of January 27th, 2018. Cygwin had a pip package available for both Python 2.7 and Python 3.x which worked for me without any sideloading or other tweaking. While this install was easier than I was expecting, given Stack Overflow responses that were considered accurate at some point in the past, those specific issues with using Python and pip through Cygwin can be considered obsolete.