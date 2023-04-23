---
title: "Cloudflare Pages Errors With CORS"
date: 2023-04-05T19:59:14-04:00
draft: false
tags:
  - Cloudflare
  - Cloudflare Pages
  - CORS
  - Hugo
---

Shortly after making my post regarding my [return to Hugo](https://looped.network/posts/back-to-hugo/) yesterday, I noticed a potential issue with my new site. While testing it from the local Hugo development server everything looked great. After linking Cloudflare Pages with the [repo](https://github.com/jfabry-noc/HugoLooped) and deploying it, though, things started to misbehave. Namely, the CSS and JavaScript weren't being loaded, giving me an _extremely_ ugly site with the `.svg` logo being the full width of the screen, a default white background, and black text with blue links. It had definite "I don't know how to scale images while making a website in the 90's" vibes.

Looking at the browser console, I was greeted with the following:

![Error in the Firefox development console saying 'None of the sha256 hashes in the integrity attribute match the content of the subresource](/2023/hugo_sha.png)

Looking at the `<head>` of my index file, I saw that the theme was providing the hashes of what the CSS and JavaScript should be. As there was a mismatch, that's why they weren't loading. Some quick searches gave me a hint with a [community post](https://community.cloudflare.com/t/cors-blocked-on-custom-domain-pages/417441) with the same problem. It seemed that using functionality of Cloudflare to automatically minify content would naturally cause problems with this setup since the content of would be altered. In that case, having the hashes not match would be expected. I'm sure I turned this feature on while playing around with a different site hosted at this same domain a long time ago and simply forgot about it. This happens because the settings for it are under the **Website** section of Cloudflare, not the **Pages** section.

I toggled the settings off in Cloudflare, let the change bake for a few minutes, and then kicked off a fresh deployment of the site. Unfortunately, the same issue persisted. I even tried removing the site entirely from Cloudflare Pages, re-linking to the repo, and doing a refresh deployment, but the same problem persisted.

Curious if this was still a Cloudflare issue or if there was a larger problem happening, I decided to try linking the repo to GitHub Pages. I added a quick [GitHub Action](https://github.com/jfabry-noc/HugoLooped/blob/main/.github/workflows/hugo.yml) for building the site and tested my first deployment. Once my domain was added and DNS was happy, everything worked smoothly. GitHub Pages is where the site is still currently at the time of this posting.

My guess is that the minify settings in Cloudflare were still the culprit, and I was simply running into a caching issue, either on the Cloudflare side or in my local browsers with which I was testing. I'd like to swap back to Cloudflare Pages at some point to test, but I also don't want to take my site down again while waiting for DNS to update. It may be easier when I get some spare time to simply make a copy of my own repository (since I can't fork it to the same account), change the base domain to what Cloudflare uses by default, and test with that. As it is, I'm currently just happy to have everything sorted out.
