---
title: "Sublime Package Control"
date: 2023-05-08T08:28:02-04:00
draft: false
tags:
  - Sublime
  - Sublime Text
  - Python
  - Black
---


Since it's been my main language for the past year, I've lately been trying to do a better job of being "Pythonic". Luckily for people like me, that's made easier by virtue of the plethora of tooling surrounding Python to at least help with things like formatting. I've started trying to be more diligent about running [Pylint](https://pypi.org/project/pylint/) against everything I write, for example.

With a [new project](https://github.com/jfabry-noc/RssToMasto) I just started working on, I decided it would be in my best interest to try formatting my code with [Black](https://pypi.org/project/black/). While Pylint will inform me of a formatting faux pas, Black will automatically fix it for me. I've been using [Sublime](https://www.sublimetext.com/) a good bit lately (with the lovely [Tokyo Night theme](https://looped.network/posts/2023/tokyo-night-sublime-text/)), and I looked for options on how to use it in an automated way with Sublime.

Given that [Package Control](https://packagecontrol.io/) is the home for all things Sublime packages, I first looked there. Sure enough, I quickly found a [package for Black](https://packagecontrol.io/packages/python-black). Awesome!

My normal means of installing packages in Sublime is to `Ctrl/Cmd + Shift + p` to open the Command Palette, type **Install**, select the option for installing from Package Control, and then type in the name of my desired package. In this case, however, I quickly ran into a problem because typing in the package name, **python-black**, didn't show me anything. There were no results. Trying to type in just **python** or just **black** and scrolling through the results also didn't reveal anything. 

I really didn't want to go with cloning the repo into `~/.config/sublime-text/Packages`, so I started digging into alternative options. Luckily, just typing **Package Control** into the Command Palette shows all of the options available. One was **Advanced Install Package**:

![The Sublime Text Command Palette showing the Advanced Install Package option](/2023/package_control_adv.png)

Selecting this option put my focus in a bar at the bottom of the screen where I was instructed to type the names of _all_ packages I wanted to install, separated by commas. On a whim, I typed `python-black` and hit Enter. Sure enough, I saw the installation go through. To confirm, I verified that in **Preferences > Package Settings** I had an option for **Python Black**. I toggled on "Format On Save", wrote a line of Python that was entirely too long, and hit Save. Black automatically reformatted the line on my behalf.

I have no idea why the package, which is still very actively maintained, didn't show up in the normal UI for finding packages, but I'm glad I was able to get it installed regardless. I figured I'd make this post in case anyone found themselves in a similar situation in the future.