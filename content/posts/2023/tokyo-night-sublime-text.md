---
title: "Tokyo Night Theme For Sublime Text"
date: 2023-04-30T16:36:45-04:00
draft: false
tags:
  - Tokyo Night
  - Sublime Text
---

I'm normally a person who changes their editor theme every few months. It's a combination of getting bored with looking at the same colors all the time and enjoying the process of hunting for new themes. I also typically use different themes across all of the editors I commonly find myself using (Neovim, Sublime Text, and VSCode) just to keep things interesting.

When I started using VSCode for C#, however, I looked for a new theme to use with it and found [Tokyo Night](https://github.com/enkia/tokyo-night-vscode-theme). I'm not sure what it is about this theme, but I absolutely love it. I wanted to use it everywhere. Fortunately for me, there's a lovely [port of it for Neovim](https://github.com/folke/tokyonight.nvim) that I started using there (and am looking at while I write this post in Neovim). I happened to fire up Sublime Text to look at a Python codebase I hadn't opened in over 6 months, however, and saw I was still using [Catppuccin](https://github.com/catppuccin/sublime-text). Catppuccin is a terrific theme, but I really found myself wanting to see Tokyo Night across everything.

I first [searched Package Control](https://packagecontrol.io/search/tokyo) and found nothing. At this point (which was my big mistake) I put in the bare minimum for searching outside of Package Control for anything relevant. When a couple of very quick searches turned up nothing, I decided my only options were:

1. Just use something else with Sublime.
2. Create my own Tokyo Night theme for Sublime.

After mulling it over and looking at [the documentation](https://www.sublimetext.com/docs/themes.html), it didn't seem like it would be **that** hard to make a theme, especially after seeing that a theme generally amounts to a JSON file.

I pulled up the [original Tokyo Night VSCode theme](https://github.com/enkia/tokyo-night-vscode-theme) to see all of the color values, and then I looked at the [Catppuccin Sublime theme](https://github.com/catppuccin/sublime-text/blob/main/Catppuccin%20Frappe.sublime-color-scheme) to steal a baseline theme template. From there I just started updating values and seeing what worked. After my initial "fill in the blanks" experience, though, it became clear that things wouldn't be quite as simple as I first thought. In a given theme, there are likely somewhere in the neighborhood of 20 colors. Looking at [an existing theme](https://github.com/catppuccin/sublime-text/blob/main/Catppuccin%20Frappe.sublime-color-scheme) there are far more properties specified. Colors are typically used multiple times, so it's important to understand what in the UI a given property in JSON refers to.

Unfortunately, this isn't as straightforward as one might hope, especially for someone like me hoping for a quick and dirty solution. After my first attempt, I would guess that about 75% of the theme was working properly. By "working properly", I mean that things were at least visible; there was still additional work to do with respect to where various colors were being applied. There were also a handful of instances where colors simply weren't specified, so they automatically received the same color as my background, making the content invisible. Other fields were clearly visible but were the flat-out wrong color, resulting in a less-than-pleasant viewing experience. I ended up working on it for a little over two hours before deciding that I needed a bit of a break.

While relaxing before diving back into my theme, I poked around the [GitHub account of the original Tokyo Night for VSCode creator](https://github.com/enkia). From this I found [an old repo](https://github.com/enkia/enki-theme) of themes for Sublime; it appears this person worked on Sublime themes prior to making VSCode themes. While the repo was quite old, one of the most recent additions was [4 years ago](https://github.com/enkia/enki-theme/commit/8ef6029e2c904f6b1dbb8bd82d6158fe83effa23) with a message of:

> Add my Tokyo Night vscode color scheme

So this awesome individual had actually back ported their theme to Sublime already. I didn't see it initially because the repo contained **all** of their Sublime themes, meaning "Tokyo Night" wasn't mentioned by name in the title. After seeing what the repo name was, I was able to quickly [find it in Package Control](https://packagecontrol.io/packages/Enki%20Theme). Suffice to say, I immediately ditched the work on my own port and simply used the one provided, which I've been loving ever since.
