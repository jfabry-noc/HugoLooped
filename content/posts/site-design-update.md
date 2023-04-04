---
title: "Site Design Update"
date: 2023-04-01T19:09:21-04:00
draft: false
tags:
  - Write.as
  - CSS
  - Dracula Theme
image: "/2023/writeas_blog_dracula.png"
---

**Note**: This post is no longer accurate since the site has moved from Write.as. However, here's a screenshot of what it looked like at the time; I was quite happy with it!

![Screenshot of this blog with a Dracula theme](/2023/writeas_blog_dracula.png)

The entire time this particular site has been in existence at [Write.as](https://write.as/), I've used the [Carbontwelve](https://write.as/themes/carbontwelve) theme for it. It's a fairly slick theme with some nice effects; I've been fairly happy with it. My only real issue is that it's a light theme, which I (more or less) never use for anything given the option. I'm not _terrible_ with CSS and have designed several decent-looking sites, both personally and for work, but it's not something I enjoy. It's more of a frustration I work to overcome and then avoid looking at again for as long as I can. As a result, I typically stick with using designs from others where possible.

Last night, however, I decided to play around with putting something together to make the design of this site something more in line with me. The nice part about Write.as is that, at least for Pro accounts, you have full control of your CSS, and it all just gets updated in one section of the settings for each blog. This means that it's simple to just copy what you have and save it, which I did as a [gist](https://gist.github.com/jfabry-noc/c7a69d69e2bb9d7031a3a48dc04441aa). Then if any changes aren't to my liking, I can just paste back what I originally had. I figured I'd do this since I had made a couple of minor tweaks from the default Carbontwelve theme that I didn't want to have to recreate.

My ultimate goal was to design the site with [Dracula theme](https://github.com/dracula/dracula-theme) colors. I used these previously in a WordPress theme I built, and I loved the way it turned out. I didn't want to have to start from scratch, though, so I looked through all of the [current themes](https://write.as/themes/) to see if there was something that would work as a base for me. I ultimately decided to use [Crop Lee](https://write.as/themes/crop-lee). It was simple enough to work my way through all of the site elements, adjusting the colors as needed. I spent a decent bit of time playing around with the formatting of code blocks so they would really pop, such as in posts like this one on [errors I saw when updating my Manjaro installation](https://looped.network/fixing-manjaro-linux-invalid-or-corrupt-package-error).

The only thing I was unhappy with was the default view when just landing at [https://looped.network](https://looped.network). I don't like the default WriteFreely view that shows the full post content in a font that's slightly smaller than the norm. I think it makes each page of posts far too long, and it isn't immediately clear to a reader that clicking through to the individual post will render it with a larger font. As someone with fairly poor vision, this sort of thing is important to me. I could easily just change the font size on the pages showing lists of posts, but that only exacerbates how long they are. In WordPress and Hugo I've always used themes that show a preview of the content, such as [Terminal](https://hugo-terminal.vercel.app/). I wanted to do something similar here.

While I had no idea how to achieve this, I didn't need to since it's something that the Carbontwelve theme does; I just needed to figure out how and copy that. After some playing around and searches to point me in the right direction of what elements may be relevant, this clever bit of CSS did the trick:

```css
body#collection #wrapper article {
    max-height: 450px;
    overflow: hidden;
    position: relative;
}

body#collection #wrapper article:before{
    content: '';
    pointer-events: none;
    width: 100%;
    height: 70%;
    bottom: 0;
    position: absolute;
    background: linear-gradient(transparent 100px, #282a36);
}
```

It essentially just sets a maximum height for the element showing each post, meaning that anything longer than this will be cut off. The gradient causes the post content to fade toward the same color as my background, indicating to the reader there's more content that just isn't visible in this particular view. Beautiful!

It ended up taking me a couple of hours to get everything tweaked the way I wanted since there was a lot time spent looking things up online and even more time with trial-and-error since no one has ever accused me of being a CSS guru. I'm really happy with how it ultimately ended up, though. If anyone wants to see more of my customizations or would like to steal the theme for themselves, it's available as a [gist](https://gist.github.com/jfabry-noc/607001f198fb5cf5a2f6551dc3b55ee5).
