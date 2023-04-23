---
title: "Real Time Error Checking and Code Completion With Neovim"
date: 2022-08-20T18:25:16-04:00
tags:
  - Neovim
  - Coc
  - Conquer of Completion
  - vim-plug
draft: false
---

I had written a few months ago on [Medium](https://loopednetwork.medium.com/state-of-vim-2022-6ad0a6722a7a) that I was trying to switch from using [VS Code](https://code.visualstudio.com/) as my main editor to [Vim](https://www.vim.org/). As I mentioned in that post, I've used Vim for _years_ now, but never as my "main" editor for when I need to get serious work done, such as with my job. I also swapped from vanilla Vim to [Neovim](https://neovim.io/), which I found to have a few quality of life improvements that I enjoyed. I just couldn't stick with it, though, because I missed the how frequently VS Code saved me from myself when I did things like making stupid mistakes that I've need to debug manually because my editor wasn't telling me about the problems in advance. Likewise, I got irritated when I kept having to check things like what parameters I needed to pass to a method or where I defined a particular class manually because I couldn't easily peek them like I can in VS Code.

That being said, I knew this functionality was possible in Neovim (and Vim), but I just never bothered to check exactly how. During some initial homework on the matter, it seemed like parts of it were fairly simple while other parts were complicated. Ultimately, it turned out that how difficult the process is to set everything up really depends on how difficult you want to make it and how much you want to customize things. I just reproduced the steps I originally followed on my work laptop with my personal laptop to validate my notes prior to making this post, and it probably took me less than 5 minutes.

## Plugins and `init.vim`

When I first started with Neovim, I quite literally told it to just use what I had already set up with Vim as far as configuration and plugins were concerned. I had used [Pathogen](https://github.com/tpope/vim-pathogen) for my Vim plugins and had my configuration done in `~/.vimrc`. Neovim looks for configuration files in `~/.config/nvim`, and they can be written in Vimscript, Lua, or a combination of the two. I initially just had my `init.vim` file with:

```vimscript
set runtimepath^=~/.vim runtimepath+=~/.vim/after
let &packpath = &runtimepath
source ~/.vimrc
```

This was taken straight from the [documentation](https://neovim.io/doc/user/nvim.html#nvim-from-vim). It worked fine, but I wanted to keep my configs separate in this case. I started my just copying the content of my existing `.vimrc` file to `~/.config/nvim/init.vim`.

**Note:** If you're curious, my full Neovim configuration is on [GitLab](https://gitlab.com/-/snippets/2391236).

Next I wanted a plugin manager. [vim-plug](https://github.com/junegunn/vim-plug) seems to be extremely popular and was simple enough to install with the command they provide:

```shell
sh -c 'curl -fLo "${XDG_DATA_HOME:-$HOME/.local/share}"/nvim/site/autoload/plug.vim --create-dirs \
       https://raw.githubusercontent.com/junegunn/vim-plug/master/plug.vim'
```

Then I just updated my `init.vim` with the plugins I wanted to install:

```vimscript
call plug#begin('~/.config/plugged')
Plug 'https://github.com/joshdick/onedark.vim.git'
Plug 'https://github.com/vim-airline/vim-airline.git'
Plug 'https://github.com/tpope/vim-fugitive.git'
Plug 'https://github.com/PProvost/vim-ps1.git'
Plug 'https://github.com/wakatime/vim-wakatime.git'
Plug 'neovim/nvim-lspconfig'
Plug 'neoclide/coc.nvim', {'branch': 'release'}
call plug#end()
```

`call plug#begin('~/.config/plugged')` and `call plug#end()` indicate what configuration pertains to vim-plug. The path inside of `call plug#begin` is where plugins get installed to; I could pick whatever arbitrary location I wanted. Plugins can be installed with any valid git link. You can see above that there's a mix of full URLs and a shorthand method. I started off by just copying the links for plugins I already used with Vim (all of the full GitHub links) and then adding the others as I looked up how to do some additional configuration. More on those later.

With `init.vim` updated, I just needed to close and re-open Neovim for everything to apply, followed by running:

```
:PlugInstall
```

This opens a new pane and shows the progress as the indicated plugins are all installed. What's really cool about this is that I can also use `:PlugUpdate` to update my plugins, rather than going to my plugin folder and using `git` commands to check for them.

### Note On Configuration

I ultimately ended up doing all of my configuration in Vimscript. I would actually prefer to use Lua, but most of the examples I found were using Vimscript. I also have a fairly lengthy function in my original Vim configuration for adding numbers to my tabs that I didn't want to have to rewrite, especially since I wholesale copied it from somewhere online. Depending on what you want to do, however, you may end up with a mix of both, especially if you find some examples in Vimscript and some in Lua. This is entirely possible. Just note there can be only **one** `init` file, either `init.vim` or `init.lua`. If you create both, which is what I initially did, you'll get a warning each time you open Neovim and only one of them will be loaded. 

To use `init.vim` as a base and then also import some Lua configuration(s), I created a folder for Lua at:

`~/.config/nvim/lua`

In there, I created a file called `basic.lua` where I had some configuration. Then, back in `init.vim`, I just added the following line to tell it to check this file as well:

```vimscript
lua require('basic')
```

## Error Checking

**Note:** I ended up not using the steps below, so if you want to follow along with exactly what I ended up using, there's no need to actually do any of the steps in this section.

This is where some options come in to play. Astute readers may have noticed the second to last plugin in my vim-plug config was for:

```
Plug 'neovim/nvim-lspconfig'
```

This is for the LSP, or Language Server Protocol. This allows Neovim to talk to various language servers and implement whatever functionality they offer. However, it doesn't actually come with any language servers included, so I needed to get those and configure them as needed. For example, I could install [pyright](https://github.com/neovim/nvim-lspconfig/blob/master/doc/server_configurations.md#pyright) from some other source, like NPM:

```shell
npm i -g pyright
```

And then I needed additional configuration to tell Neovim about this LSP. The samples were in Lua, which is why I initially needed to use Lua configuration alongside Vimscript:

```lua
require'lspconfig'.pyright.setup{}
```

This actually worked for me with respect to error checking. Opening up a Python file would give me warnings and errors on the fly. However, I didn't get any code completion. I started looking at options for this, but frankly a lot of them seemed pretty involved to set up, and I wanted something relatively simple rather than having to take significant amounts of time configuring my editor any time I use a new machine or want to try out a different language.

## Code Completion

Ultimately, I stumbled onto onto [Conquer of Completion](https://github.com/neoclide/coc.nvim), or coc. I don't know why it took me so long to find as it seems to be _insanely_ popular, but better later than never. One of coc's goals is to be as easy to use as doing the same thing in VS Code, and I honestly think they've nailed it. I first installed it via vim-plug in `init.vim`:

```
Plug 'neoclide/coc.nvim', {'branch': 'release'}
```

After restarting Neovim and running `:PlugInstall`, I could now install language servers straight from Neovim by running `:CocInstall` commands:

```
:CocInstall coc-json coc-css coc-html coc-htmldjango coc-pyright
```

After this, I fired up a Python file and saw that I had both error checking **and** code completion. There was just one final step.

## Key Mapping

Given the wide array of key mapping options and customizations that people do, coc doesn't want to make any assumptions about what key mappings are available and which may already be in use. As a result, there are NO custom mappings by default. Instead, they need to be added to your Neovim configuration just like any other mapping changes. However, the project shares a terrific example configuration with some recommended mappings in their [documentation](https://github.com/neoclide/coc.nvim). I legitimately just copied the sample into my existing `init.vim` file. This adds some extremely useful mappings like:

- `gd` to take me to the declaration for what I'm hovering.
- `K` to show the documentation for what I'm hovering (based on the docstring for Python, for example.)
- `]g` to go to the next error/warning and `[g` to go to the previous one.
- `Tab` and `Shift + Tab` to move through the options in the code completion floating window.
- `Enter` to select the first item in the code completion floating window.
- A function to remap `Ctrl + f` and `Ctrl + b`, which are normally page down and page up, to scroll up and down in floating windows but only if one is present.

And tons of other stuff great stuff. I initially spent about 30 minutes just playing around with some throwaway code to test all of the different options and key mappings. It honestly feels super natural and now gives me the same benefits of VS Code while allowing me to use a much leaner and more productive editor in Neovim.
