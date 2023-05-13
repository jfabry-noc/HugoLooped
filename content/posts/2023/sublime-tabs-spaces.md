---
title: "Sublime Text Converting Tabs To Spaces"
date: 2023-05-13T08:34:48-04:00
draft: false
tags:
  - Sublime
  - Sublime Text
---

As I mentioned in a [recent post](https://looped.network/posts/2023/sublime-package-control/), I've been using Sublime Text a decent bit. One issue I discovered myself running into was a conflict between tabs and spaces in my code. Running `pylint`, for example, would give me output like the following:

![pylint output showing errors related to the number of spaces](/2023/pylint_bad_indent.png)

Given that Python is based around indentation, this isn't an uncommon scenario, and one that I would typically rectify easily with the following command in Neovim:

```
:retab
```

In fact, opening the relevant files in Neovim and running the above command prior to re-running Pylint would resolve the error. Since Sublime will, by default, display whitespace characters when they're highlighted, I could easily see what was going on. Highlighting a tab would show one contiguous tab character. This mirrored what `pylint` reported with there being one character rather than 4. I needed my tabs to be 4 spaces. This initially seemed simple. I just opened my Sublime settings and added the following line:

```
"translate_tabs_to_spaces": true
```

After saving my settings, I deleted a tab in question and hit the Tab key again. However, I _still_ got a tab character rather than 4 spaces. Odd.

I took to the Internet for some additional searches and eventually came across a [Stack Overflow post](https://stackoverflow.com/questions/38770226/sublime-text-3-translate-tabs-to-space-feature-doesnt-work) which said it may be necessary to close each individual _file_ and then re-open each of them. I did just that, closing every open file in the project prior to closing and relaunching Sublime, which I typically do from the CLI in the root of my project's directory via:

```bash
subl .
```

After going through this process, I went back to a line in question, deleted the single tab character, and then validated hitting the Tab key on my keyboard inserted the expected 4 spaces in stead. Re-running `pylint` showed that everything was happy. This also resolved a problem I was having where changes implemented by [Black](https://packagecontrol.io/packages/python-black) would result in issues due to mismatched indentation.
