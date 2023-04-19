---
title: "Fedora 38 Release"
date: 2023-04-18T20:22:04-04:00
draft: false
tags:
  - Fedora
  - Linux
---

I saw on [Mastodon](https://social.sdf.org/@fedora@fosstodon.org/110220343946534501) earlier today that Fedora 38 was released. I had checked several weeks ago on when it was expected, and while I knew it was going to be soon, it had fallen off my radar while I've been preoccupied with a few other things. While I tend to be a bit more cautious with my work machines, on my personal machines I typically pull the trigger on major new OS releases with a fairly cavalier attitude, so as soon as I had a moment I went to get the upgrade rolling.

## First Try

The [official Fedora docs](https://docs.fedoraproject.org/en-US/quick-docs/upgrading/) recommend that you upgrade "Workstation" versions of Fedora via the Software UI. I thought I read once upon a time that when you manage any software through this application there's some extra magic happening under the hood compared to just running `dnf`, but I could be mis-remembering and never bothered to look into matters further. Regardless, as the recommended option, that's what I tried first. I've used this method for the the last few Fedora upgrades ever since I swapped to Fedora from Pop!_OS and never had issues. Unfortunately, things didn't want to cooperate this time.

The first thing I did was go to the Terminal and do a full upgrade of any existing packages via:

```shell
sudo dnf upgrade --refreshsudo dnf upgrade --refresh
```

This worked fine and prompted for a reboot, which I did. Next I went to the Software utility, checked for updates through that, and saw the expected prompt to download Fedora 38. After clicking the button to kick off the download, things went by for a few minutes, with the percentage climbing up to around 22% until the application abruptly took me back to the download prompt as if I hadn't already started the process. Clicking the download button again caused the process to almost _immediately_ jump to 22% before again returning to the download prompt. Weird.

I tried giving my system another reboot and repeating the process. Unfortunately, I still saw the same behavior with an almost immediate jump to 22% before everything reset itself.

## Second Try

At this point I had the option to either:

1. Research what was going on through the UI.
2. Upgrade my system from the CLI.

I opted for the second simply because I'm already more used to upgrading most of my headless Linux systems from the CLI, so it just feels natural. Most of my other systems are either Debian-based or Arch-based, though, so this experience would be a bit different for me. Luckily, it's [well documented](https://docs.fedoraproject.org/en-US/quick-docs/dnf-system-upgrade/).

The first step was to make sure the utility for doing OS release upgrades via `dnf` was installed:

```shell
sudo dnf install dnf-plugin-system-upgrade
```

After that I had to download Fedora 38:

```shell
sudo dnf system-upgrade download --releasever=38
```

And then tell my system to reboot and begin the upgrade process:

```shell
sudo dnf system-upgrade reboot
```

The upgrade process only took a few minutes, and before long I was logged back into my machine and verified it was running Fedora 38:

```shell
cat /etc/os-release
```

The aforementioned documentation mentioned a few cleanup activities worth doing. These ones were relevant for me. First, I checked for packages that had been installed in Fedora 37 but were now retired in Fedora 38:

```shell
sudo dnf install remove-retired-packages
sudo remove-retired-packages
```

I next checked for any packages with broken dependencies and packages with multiple versions. I didn't have any of either:

```shell
sudo dnf repoquery --unsatisfied
sudo dnf repoquery --duplicates
```

Lastly, I checked for any broken symbolic links. I actually had a mountain of these, which after reviewing in the first command I deleted with the second:

```shell
sudo symlinks -r /usr | grep dangling
sudo symlinks -r -d /usr
```

And that's all there was to it! I've been using my laptop for the past few hours, and everything has been working quite well. Fedora Magazine did a [succinct write-up of the changes](https://fedoramagazine.org/announcing-fedora-38/).
