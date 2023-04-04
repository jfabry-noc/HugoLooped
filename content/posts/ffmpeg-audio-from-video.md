---
title: "Extracting Audio From Video With FFmpeg"
date: 2022-09-14T18:28:19-04:00
tags:
  - ffmpeg
  - MP3
  - MP4
draft: false
---

Just a few moments ago I needed to extract the audio component out of a video file into some type of standalone audio file, like `.mp3`. Since I've been working with [Audacity](https://www.audacityteam.org/) to _record_ audio, I figured maybe it had some capability for ripping it out of video.

My initial searches gave me results like [this](https://www.youtube.com/watch?v=SP8IONsqMVk) which quickly made it clear that while this is technically possible, it requires some add-ins that I didn't really want to mess around with. However, since the add-in mentioned in that video was for [FFmpeg](https://ffmpeg.org/), I realized I could just use that directly.

I didn't have `ffmpeg` installed, but that was easy enough to rectify on Fedora 36.

```shell
sudo dnf install ffmpeg-free
```

Then I needed to extract the audio. I first checked how it was encoded in the video with:

```shell
ffprobe my_video.mp4
```

After sifting through the output, I saw that it was encoded as **aac**:

> Stream #0:1[0x2](eng): Audio: aac (mp4a / 0x6134706D), 48000 Hz, stereo, s16, 317 kb/s (default)

Rather than that, I wanted to simultaneously re-encode the audio as MP3. Another quick search showed me some [great resources](https://stackoverflow.com/questions/9913032/how-can-i-extract-audio-from-video-with-ffmpeg). Ultimately, I ended up doing:

```shell
ffmpeg -i my_video.mp4 -q:a 0 -map a bourbon.mp3
```

As mentioned in the [Stack Overflow post](https://stackoverflow.com/a/36324719), the `-q:a 0` parameter allows for a variable bitrate while while `-map a` says to ignore everything else except the audio.

Just a few moments later, and my MP3 was successfully encoded.
