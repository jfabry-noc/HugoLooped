---
title: "Fixing Gnome Software Not Loading Any Data In Fedora"
date: 2023-05-12T10:18:10-04:00
draft: false
tags:
  - Gnome Software
  - Fedora
  - Linux
---

I recently wanted to install [Steam](https://store.steampowered.com/) on my Fedora Linux laptop. A quick search on the proper way to do so showed that it [could be done via RPM Fusion](https://itsfoss.com/install-steam-fedora/). Instead of following the CLI steps, I decided to be lazy and just use Gnome Software to do it via a UI. Unfortunately, I ran into a problem that I frequently stumble across with Gnome Software; it doesn't actually load any content. The landing page will show the different software categories and a rotating banner of software at the top of the screen. Clicking on any category or a piece of software from the banner will result in a blank page that _looks_ as though it's trying to load content, but it will never do anything. Similarly, trying to do something like check for updates or (as was needed in this case) look at the currently enabled repositories will also result in an eternal loading icon.

Rather than just ignoring the problem and going back to the CLI like I normally do, I decided to try getting to the bottom of the problem. Fortunately, the fix was quite easy. I imagine this should be relevant on any system with Gnome and not just Fedora, but Fedora is what I used it on. I also didn't come up with this myself; it was the first thing listed on this [article](https://www.linuxuprising.com/2019/03/fix-gnome-software-stuck-with-software.html).

The first step is to make sure there are no instances of Gnome Software running:

```shell
killall gnome-software
```

Next, just kill the local cache for the application:

```shell
rm -rf ~/.cache/gnome-software
```

After that, relaunch Gnome Software. For me, it took a couple of seconds to load the software catalog fresh before displaying the landing page. After that, however, I was able to quickly check which repos were enabled, see that I had apparently already enabled RPM Fusion at some point, and then install Steam without any issues.