---
title: "Fixing Manjaro Linux Invalid Or Corrupt Package Error"
date: 2022-09-17T18:30:30-04:00
draft: false
---

As the title alludes to, this morning I tried updating my [Pinebook Pro](https://www.pine64.org/pinebook-pro/) running Manjaro Linux through my normal method:

```shell
sudo pacman -Syu
```

Today, this resulted in an error message about the `libibus` package:

> warning: could not fully load metadata for package libibus-1.5.26-2
error: failed to prepare transaction (invalid or corrupted package)

Fun. I first wanted to see if it was really _just_ this package that was causing the problem or if there were other issues. Being a `pacman` noob, I just used the UI to mark updates to the `libibus` package as ignored. Once I did that, all of the other packages installed successfully. This prompted for a reboot, which I gladly did since I figured I'd see if that made any difference. Once my laptop was back up and running, though, executing `pacman -Syu` again still gave the same error related to `libibus`.

Some searches online showed that a mirror with a bad package could be the problem, so I updated my mirrors via:

```shell
sudo pacman-mirrors -f5
```

This didn't solve the problem, but the new mirror gave me a different error message:

> error: could not open file /var/lib/pacman/local/libibus-1.5.26-2/desc

With some more searches online, I saw a few people on the Manjaro forums say that simply creating those files was enough to fix similar errors they had with other packages. Creating the file above just resulted in an error about a second file being missing, so I ultimately ended up running:

```shell
sudo touch /var/lib/pacman/local/libibus-1.5.26-2/desc
sudo touch /var/lib/pacman/local/libibus-1.5.26-2/file
```

Now running an update allowed things to progress a little further, but I got a slew of errors complaining about `libibus` header files (`.h`) existing on the filesystem. My next less-than-well-thought-out idea was to just remove the package and try installing it fresh. I tried running:

```shell
sudo pacman -R libibus
```

Fortunately, Manjaro didn't let me do this by telling me that it was a dependency for `gnome-shell`. Yeah, removing that would've been bad. It was back to searching online. The next tip I stumbled across was to try clearing the `pacman` cache and then install updates with:

```shell
sudo pacman -Scc
sudo pacman -Syyu
```

This unfortunately gave me the same error about the header files. However, the same [forum thread](https://forum.manjaro.org/t/cant-update-manjaro-invalid-or-corrupted-package/78802) had another recommendation to run:

```shell
sudo pacman -Syyu --overwrite '*'
```

Curious about exactly what this would do prior to running it, I checked out the `man` page for `pacman`:

> Bypass file conflict checks and overwrite conflicting files. If the package that is about to be installed contains files that are already installed and match glob, this option will cause all those files to be overwritten. Using --overwrite will not allow overwriting a directory with a file or installing packages with conflicting files and directories. Multiple patterns can be specified by separating them with a comma. May be specified multiple times. Patterns can be negated, such that files matching them will not be overwritten, by prefixing them with an exclamation mark. Subsequent matches will override previous ones. A leading literal exclamation mark or backslash needs to be escaped. 

I took this to mean that instead of complaining about the header files that already existed on the filesystem, it would simply overwrite them since my glob was just `*` to match _anything_. I ran this, and sure enough everything was fine.

I mainly run Manjaro on my Pinebook Pro just because it's such a first class citizen there with tons of support. It's now the default when new Pinebook devices ship; back when I got mine it was still coming with Debian, though I quickly moved it over after seeing how in love the community was with Manjaro. I do find that I run into more random issues like this on Manjaro than I do with Fedora on my other laptop or Debian on my servers, for example, and at times it can be a little frustrating. I didn't really want to spend a chunk of my Saturday morning troubleshooting this, for example. But while there seem to be more issues with Manjaro, the documentation and community are so good that usually after a little time digging in, the solution can always be found. I've yet to run into any issue where the [current installation was a lost cause](https://loopednetwork.medium.com/pop-os-upgrade-issues-e8bc95935ff8) forcing me to reinstall the operating system.
