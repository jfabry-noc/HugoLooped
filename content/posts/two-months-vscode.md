---
title: "Two Months With VSCode"
date: 2023-04-08T18:07:57-04:00
draft: false
tags:
  - VSCode
  - Vim
  - Editor
---

As I've [written about before](https://looped.network/posts/neovim-lsp/), Vim (specifically [Neovim](https://neovim.io/)) is my editor of choice. I use it with Python, Go, Groovy, and to write these posts in Markdown. I find Vim's modal setup to be the most efficient way to edit text, and after becoming (moderately) proficient with the motions I feel like I'm significantly faster than if I had to bother with arrow keys and various combinations of Command, Ctrl, etc.

However, for the past couple of months I've spent the majority of my time writing C# to create a binary PowerShell module. This also caused me to start my latest [personal project](https://github.com/jfabry-noc/TootSharp) in C# since I'm relatively new to it and wanted to get as much experience as I could. While [OmniSharp](https://www.omnisharp.net/) exists and actually forms the basis for the [C# extension in VSCode](https://marketplace.visualstudio.com/items?itemName=ms-dotnettools.csharp), I didn't have a great deal of success using OmniSharp by itself. It _worked_, but I wanted a lot more out of it, especially for a language I was trying to learn at the same time. I tried writing some C# with VSCode and had much better success. As a result, I bit the bullet and started using VSCode any time I was writing C#.

I previously used VSCode pretty much from when it was released until late 2019. During that time I was working as a sysadmin in a Windows-centric environment, meaning that PowerShell was the overwhelming majority of what I wrote and VSCode just seemed natural as it replaced the [PowerShell ISE](https://learn.microsoft.com/en-us/powershell/scripting/windows-powershell/ise/introducing-the-windows-powershell-ise?view=powershell-7.3). However, when I took a new role in 2019 and began working in Python and Groovy, I started using Vim instead and became fairly enamored with it. I figured as a Vim aficionado I'd share some thoughts on coming back to VSCode for the past few months.

## Pros

The biggest benefit is that, given how ubiquitous it's become, basically **everything** imaginable just _works_ with it. You install an extension with the click of a button, wait a few seconds, and suddenly you have an LSP, syntax support, debugging, etc. all ready to go. The same goes for new supporting tech, like [GitHub Copilot](https://github.com/features/copilot). While Copilot definitely works with Neovim as well, I found the experience to be a bit more polished in VSCode.

Debugging in particular was extremely easy, and quickly made a big difference as I worked through some troublesome issues that would've been much more difficult to get to the root of without it. I know you can do very similar things in Vim, but it's a massive pain to get everything set up, make sure you have something bound for setting and unsetting breakpoints, etc. (Note: "massive pain" is obviously relative. If you're more proficient in configuring Vim than I am it may be a breeze.)

Out of the box, VSCode has the "normal" text editing experience, but the [Vim extension](https://marketplace.visualstudio.com/items?itemName=vscodevim.vim) allows it to become a modal editor in its own right with the majority of the motions I'm used to available out of the box. This wasn't perfect, though, as I'll touch on in the next section.

VSCode also has themes for basically any palette I can imagine, again owing to its immense popularity. I ended up swapping themes a handful of times in just a matter of months not because there was anything wrong with the one I had been using but because there were so many options. Once again, Vim has a ton of themes as well, and there's almost always overlap between the two. I think discovering themes is a little easier with VSCode, though, and as per the norm everything just works when you install the theme extension. Vim often requires tweaks to my config file based on the theme to ensure I'm getting the right palette, color support, italics support, etc.

I had a love-hate relationship with the terminal integrated into VSCode. While I found it a bit clunky for most tasks, it was useful for quickly jumping to a CLI, opening a PowerShell session, compiling and importing my module, and then testing whatever functionality I had just been working on. This became even nicer after I discovered that **Ctrl + `** would toggle focus from the editor to the terminal. Hitting it a second time with the terminal in focus would shift focus back to the editor and _minimize_ the terminal, which I highly appreciated.

The ability to work with non-code files in a meaningful way was extremely nice. Along with the README for my repos, I often write various other documentation in Markdown. Being able to quickly preview what that would look like inside of the editor was very handy, as was the fact that I found [an extension](https://marketplace.visualstudio.com/items?itemName=yzane.markdown-pdf) which would allow me to convert that Markdown to a PDF in the instance I was sharing it with customers. Likewise, much of my coding involves REST APIs, so I'm frequently looking at raw JSON responses. Being able to paste that into a file in VSCode, right-click, and select to format the document into a whitespace-friendly version was extremely nice. Previously I would end up having to paste the same JSON into something like [JSONLint](https://jsonlint.com/).

## Cons

The most immediate con to VSCode, especially as someone who mainly uses Vim, is that it is unbelievably slow while being a massive drain on system resources. Even on a MacBook Pro spec'd out for development, it would lag with a suprising degree of frequency while consuming several gigabytes of RAM. I was initially curious if this was a problem specific to C#, so I tried writing a little Python with it. This experienced the same issues; fixing a warning from Pylint and re-saving the file would often require 5+ seconds for the warning to be removed. I didn't expect performance to be amazing since VSCode is an [Electron](https://www.electronjs.org/) application, after all, but I also didn't expect it to be quite as bad as it is.

While the aforementioned Vim extension makes editing tolerable, I found that was only the case on my MacBook simply because there's less overlap in commands between Vim and VSCode itself. For example, on my MacBook I can use **Cmd + f** to open a search because it doesn't overlap with anything. On my Linux laptop, though, hitting **Ctrl + f** to do the same was problematic depending on what mode I happened to be in. For example, if I was in Normal mode, **Ctrl + f** would take the Vim action of jumping forward one page. I had to swap to Insert mode and _then_ hit **Ctrl + f** to search. It's not a big deal, but it was enough to be annoying.

Since the time I last used it, VSCode has become even more of a one-stop-shop for developers, with out of the box integrations for all sorts of things. While that could be helpful if I found them useful, for anything I didn't use it was just an additional drain. For example, VSCode has support for managing a git repository from right within the editor. I tried it out but ultimately found myself preferring to just run `git` commands from a terminal the way I normally do. While having extras doesn't necessarily hurt anything (aside from eating up yet more resources), it reminds me a bit of back when Opera was a web browser that also had an email client, IRC client, etc.

## Wrap-up

Would I consider switching from Vim to VSCode? While I wouldn't be overly bothered if I found myself in an environment where I _had_ to use VSCode, I don't think I'd end up swapping to it for my non-C# work. I just value the speed and efficiency of Vim a little too much at the moment. If VSCode was a little bit leaner, though, I could see it becoming a more attractive option.
