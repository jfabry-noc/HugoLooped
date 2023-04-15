---
title: "Pylint Import Error"
date: 2023-04-15T15:57:30-04:00
draft: false
tags:
  - pylint
  - Python
---

I recently found myself quickly checking in on a Python project outside of my normal [Neovim](https://looped.network/posts/neovim-lsp/) workflow. I was basically just trying to quickly re-familiarize myself with some code that I hadn't looked at or thought about in a few months before deploying it for a customer. If I'm feeling particularly lazy when doing something like this, I'll often open something up in [Sublime](https://www.sublimetext.com/) rather than Neovim just because it allows me to quickly scroll through a project and hop between files. I frequently do the same thing when I'm walking through some code on a screen share just because a GUI editor lends itself to that better, in my experience. While I don't use Sublime as my main editor, I do have it fairly configured, including with [pyright](https://packagecontrol.io/packages/LSP-pyright), so that I'm warned about problems regardless of the circumstances under which I'm opening the project.

I was surprised, however, when Sublime complained that it couldn't find a 3rd party library for my `import` statement. After I validated the project had a `venv` created and the `venv` contained the package I was importing, I quickly determined that the problem was related to my tooling rather than the code itself; the code executed fine in my lab environment. In fact, from the `venv` I could launch the Python REPL and import the library in question without issue. Opening the same project in both Neovim and VSCode didn't result in the same warning. Going directly to the source, I simply ran `pylint` manually from the CLI. Sure enough, I received the same error:

![Sceenshot of pylint reporting that it's unable to import a library](/2023/pylint_import.png)

This next part took me longer than I care to admit to figure out, but I wasn't sure why Sublime and `pylint` directly couldn't find the packages while Neovim and VSCode could. Neovim typically operates by just looking at what's in the `venv` from the context where it was launched. VSCode makes me specify which `python` executable I want to use for the project. Similarly, for pyright to work in Sublime, I need to create a `pyrightconfig.json` file in the root of the project's directory. This can be done via the command palette, and at its simplest it just tells pyright what the `venv` is:

```json
{
    "venvPath": ".",
    "venv": ".venv"
}
```

I add this to basically **all** of my Python projects regardless of whether or not I'm ever going to open it in Sublime just to make my life easier in the event that I do. If the package was in the `.venv` directory, why couldn't `pylint` see it?

The answer that many readers may have guessed already is that at some point I had accidentally installed `pylint` **globally**. Running:

```shell
which pylint
```

Showed me that it was installed at: `/usr/local/bin/pylint`

Oops. Given that I had Python 3.9, 3.10, and 3.11 on the same machine, I had to do some quick trial and error to figure out which one had been used to erroneously install `pylint` so that I could then remove it. Once that was done, I just installed it _locally_ within my project's `venv` and all was right with the world. Both `pylint` itself and Sublime were happy to see the library I was attempting to import. As an interesting side note, while Sublime threw a warning about its inability to find the library being imported, the `pyrightconfig.json` file was still working since autocompletion worked via pyright.
