---
title: "Hugo Yearly Post Sub-Directories"
date: 2023-04-23T17:56:28-04:00
draft: false
tags:
  - Hugo
  - SSG
  - Static Site Generator
---

## Yearly Post Directories

I recently updated this site to place all of the posts in yearly sub-directories. So instead of my posts being something like:

`/posts/back-to-hugo/`

They're now:

`/posts/2023/back-to-hugo/`

I did this mostly for my own sake. While the site is small, if I keep using it for a while then the [posts](https://github.com/jfabry-noc/HugoLooped/tree/main/content/posts) directory in my git repository would end up being fairly massive. Along with being ugly, that just makes it more difficult for me to do things like:

1. Find the relevant file.
2. Name things uniquely.

There were just a couple of things I needed to do to make this work. First, in my `config.toml` file I added a list for `mainSections` under `[params]`:

```toml
mainSections = ["2022", "2023"]
```

Next I manually created a `2022` and a `2023` directory in `content/posts` and moved my Markdown files from the root of the `posts` directory to the appropriate yearly directory. Locally testing this via...

```shell
hugo server
```

... showed that everything looked good. A commit to the repository later, and everything was live on the new site. It's definitely worth mentioning, just in case it isn't obvious, that this _will_ break any existing links since my post URLs now contain the year. For me, this was desirable. Given that the site isn't particularly large, I figured it wasn't a big deal.

I had been curious about the process for creating new posts since I would previously just do:

```shell
hugo new posts/file-name.md
```

Now I just specify the full path:

```shell
hugo new content/posts/2023/file-name.md
```

## Removing Public Directory From Origin

One mistake I had been making was that I had committed my `/public` directory to my repository, so the compiled content of the site was included in GitHub. That was fairly useless since I have a [GitHub Action](https://github.com/jfabry-noc/HugoLooped/blob/main/.github/workflows/hugo.yml) to compile the site when a new commit is pushed; my `/public` directory in origin was just a lot of static content that wasn't being used for anything.

I removed the directory from origin via the trusty:

```shell
git rm -r --cache public/*
```

Note that if you are okay to delete the local content as well, the `--cache` flag can be omitted. I next added that directory to my `.gitignore` file so that any subsequent content which may end up there wouldn't accidentally be included when staging files. My initial thought was that I would still want to keep compiling the site locally to review changes, new posts, etc. prior to pushing to GitHub. However, I realized afterward that's not quite right since running `hugo server` will compile the content into memory, which is what allows it to render new changes on the fly without re-running `hugo` after a file has been updated and saved.

Another nice benefit to this is that, after updating the site, adding a new post, etc. locally, it's a lot easier to tell what has changed with:

```shell
git status
```

Prior to this, I'd end up with at least a few dozen files that were new, updated, etc. in `/public`. It was simple enough to sort through, but it was still more effort than was necessary. Without running `hugo`, the only items which show up when I need to check what files have been modified are the ones I manually modified.

Now my repo's origin is a bit slimmer and doesn't contain wasted files. Likewise, I can remove running `hugo` from my workflow entirely.