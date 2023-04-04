---
title: "Back to Hugo"
date: 2023-04-04T18:12:18-04:00
draft: false
tags:
  - SSG
  - Static Site Generator
  - Hugo
  - Cloudflare
  - Cloudflare Pages
---

After a little over a year with [Write.as](https://write.as), this site is once again being built with [Hugo](https://gohugo.io/), my favorite static site generator. I'm currently hosting it through [Cloudflare Pages](https://pages.cloudflare.com/). I don't know that Cloudflare is any better or worse than other static hosts out there, but since I already use them for a handful of things (including hosting another site), I figure it's easier for me to just keep everything in one place.

There are a few reasons for the change, which is probably a bit odd given that I _just_ [updated the design](https://looped.network/posts/site-design-update/). One is that while Write.as is a **great** service--I honestly have nothing but good things to say about it--it's still a small project run by a single person. What happens to the service if something happens to that person, if that person decides to move on to something else, etc? While it is easy enough to export my data from Write.as, it's still simpler to get it somewhere else while I still have a fairly small number of posts at it; when I started there I didn't bother to move most of my _really_ old content that I just backed up at [Medium](https://loopednetwork.medium.com/).

Another reason is that, for reasons I'll cover in another post at another time, I'm once again circling back to taking stock of all of my various subscriptions. I actually did a [podcast about this exact problem](https://sameshadeofdifference.com/episodes/mo-money-mo-money) (back when I was still podcasting.) When subscriptions are relatively cheap, it's easy to just not think about them and how they all add up. In this case, timing is fortuitous as I had _just_ received the notice that my Write.as subscription would be renewing; it's due on the 10th. That's $72 that I don't really **need** to spend since I can easily build and host the site for free. Having that done for me was a matter of convenience that may not necessarily be worth the price of admission.

When it came to picking a theme, I stuck with some of the key things I discussed in my post about redoing my Write.as theme. I wanted something dark that would allow me to only show a preview of each post rather than having the full content of every post displayed on each page. While I was tempted to go with the tried-and-trusted Terminal theme that I've used many times before, this time I decided to try [Ficurinia](https://themes.gohugo.io/themes/hugo-ficurinia/). The default look is quite nice, though it offers a ton of customization options if I eventually want to tweak it a little bit.

My only real issue with it is that I the link to my [Mastodon](https://mastodon.online/@looped_network) account doesn't include the `rel="me"` part that's required for my site to show up as being verified:

```html
<a rel="me" href="https://mastodon.online/@looped_network">Mastodon</a>
```

I may see if I can figure out how to add that to the theme and, if so, open a PR for it.
