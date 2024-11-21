---
title: my zsh configuration 101 (in Arch Linux)
author: pienapin
date: 2024-11-21 12:40:00 +0700
categories: [guide]
tags: [linux, shell, zsh, technical]
pinned: true
toc: true
---

## The Why
---
So, my desktop pc is broken and it's been a week or two. It forces myself to use my old laptop (Asus X555L) with i5 gen 5, a Geforce 940M, and 8 Gigs of DDR3 RAM. It is not that bad actually and i can manage with it, but.. the hinges and some parts of the frame between the laptop's machine/board and its display is gone. I have to use monitor but sadly i can only use one.

Anyway, i decided to install Windows 10 (AtlasOS) on it as the main system and install Arch Linux in a Virtual Machine. I haven't really backup/push my latest commit of dotfiles so here we are, configure stuff.. again.

Starting from shell, i use zsh instead of bash because AFAIK zsh has more features than bash. Zsh has better completion, tab completion, many options for themes and plugins, and probably many more. While bash itself is not bad at all, it doesn't hurt to get zsh.

For the prompt of the shell, i choose starship because it is the minimal, blazing-fast and infinitely customizable prompt for any shell! yeah, that is the tagline.. and also rust. Another choice is spaceship which is basically starship that is exclusively developed for zsh, but i think starship is a little bit faster than spaceship.

## The Requirements
---
* zsh
  * zsh-completions
  * zsh-antidote (plugin manager, making your life easier)
* Starship
* Package manager / AUR Helper (i use paru)
* Any Nerd Fonts (for starship)
* Text Editor (for configuration)

## The Steps
---
1. Install all the packages for zsh
```console
paru -S zsh zsh-completions zsh-antidote
```
2. Change shell to zsh
```console
chsh -s $(which zsh)
```
3. Re-login to your display manager
4. Configure the zsh or just leave the .zshrc alone, it wont matter as we are going to overwrite the config
5. Make a directory for zsh config
```console
mkdir -p .config/zsh
```
6. Create a `~/.zshenv` file which contains
```
export ZDOTDIR="$HOME/config/zsh"
```
7. Create a `~/.config/zsh/.zshrc` file and put in lines below
     
    ```text
    HISTFILE=~/.config/zsh/history
    HISTSIZE=10000
    SAVEHIST=2500
    bindkey -v
    
    zstyle :compinstall filename '/home/username/config/zsh/.zshrc'
    zstyle ':completion:*' menu select
    
    autoload -Uz compinit
    compinit
    ```
  
    That is the basic configuration which sets the history file, auto completions, and select completion.
     
8. Clone the zsh antidote into the zsh config directory
```console
git clone --depth=1 https://github.com/mattmc3/antidote.git ${ZDOTDIR:-~}/.antidote
```
9. Create a `~/config/zsh/zsh_plugins.txt` which contains the plugins that we want to install. For example,
```
zsh-users/zsh-autosuggestions
zdharma-continuum/fast-syntax-highlighting kind:defer
```
10. Load the antidote inside the `.zshrc` file by adding lines below
```
source ${ZDOTDIR}/.antidote/antidote.zsh
antidote load ${ZDOTDIR}/zsh_plugins.txt
```
11. Open a new terminal, and plugins will be automatically downloaded and loaded.
12. Install starship prompt
```console
paru -S starship
```
13. Load by adding this line to the end of the file of .zshrc
```
eval "$(starship init zsh)"
```
14. Configure starship in `~/.config/starship.toml`
That's it! Now we can add more configuration that we want easily.
