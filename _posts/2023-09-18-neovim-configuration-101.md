---
title: neovim configuration 101
author: pienapin
date: 2023-09-18 10:15:00 +0700
categories: [guide]
tags: [neovim, text editor, configuration, linux, technical]
pinned: true
toc: true
---

## The Why
---
I love the looks of terminal. CLI is one of the most amazing thing in Linux. In fact, that is one of the reason on why I use Arch Linux on my devices.

In using arch linux, it is a must to configure it so that I can use it like a normal person. To configure it, I saw a lot of tutorials and most people use text editor in terminal to edit the configuration files.

After using a lot of different text editor, I prefer neovim as my text editor in terminal. Mainly, because nano is too simple and emacs is too complicated.

Well, why not vim? After a little bit of reading, there is more sources saying 'neovim is improved vim' instead of 'neovim is unnecessary, vim is more than enough'. So, i choose neovim.

But after years using Arch Linux, I couldn't never use neovim to replace VSCode even i wanted to. Thus, I only use neovim to edit my configuration files. I realize that is because my neovim is not configured enough for me to use it as a replacement for VSCode.

So with this, I want to configure my neovim for once and documented it for myself.

## The Could Have (Alternative Ways)
---
Instead of configuring neovim, I could have just keep using VSCode, but the thing is I want to try configure my neovim enough that i can use it instead of VSCode at least once.

Instead of using neovim, I could have just use Spacevim or Lunarvim which is vim/neovim but is already configured. But, i have already tried that and i got overwhelmed since it is already configured too much that it became complicated for me.

In addition, I found a text editor called 'Onivim' while writing this. It looks like mix of VSCode and vim, it even able to install plugins from VSCode. I might try it later.

## The Requirements
---
* Device with system that can install and use neovim

## The Guides
---
### Basic Configuration
1. Install neovim
2. Create a directory for neovim configuration in .config
    ```console
    mkdir -p .config/neovim
    ```
3. Create a file inside which is the main file that will be read by neovim
    ```console
    cd .config/neovim
    touch init.lua
    ```
4. Create a directory named `lua` and create four files inside

    ```console
    mkdir lua
    touch options.lua
    touch plugins.lua
    touch manager.lua
    touch keymaps.lua
    ```

    So the idea here is, we will write the configuration in that four files. `options.lua` contains basic/general configuration of the neovim such as number, tab stop, etc.
    `plugins.lua` like its name, contains list of plugin which we are going to use.
    `manager.lua` is a lua file that contains configuration for lazy.nvim which is a plugin manager that we will use.
    `keymaps.lua` contains configuration for keymaps.

5. Write lines below to `init.lua`
    ```lua
    require "options"
    require "manager"
    require "keymaps"
    ```
6. Write basic configuration for neovim in `lua/options.lua`
    ```lua
    vim.o.clipboard = 'unnamedplus'
    vim.o.number = true
    vim.o.tabstop = 2
    vim.o.shiftwidth = 2
    vim.o.shiftround = true
    vim.o.termguicolors = true
    vim.o.updatetime = 300
    ```

    Basically, clipboard is to select which clipboard the neovim will use, you can choose between unnamed or unnamedplus. unnamedplug will allow you to copy-paste from and to neovim from other program, hence why i use it. Next number, like the name of the option, it will enable number in neovim. You can choose between regular number or relativenumber, the difference is relativenumber will show number of lines relative to current line where the cursor is. It will be easier to navigate but i havent get the hang of it, so i use regular number.

    tabstop configure how much space a `TAB` will give, while shiftwidth configure how much space a `>`, `<`, and `=`, which are keybinds to indent lines, will give. by enabling shiftround, the indent value will be round to multiple of shiftwidth value and make the code indentation cleaner.

    termguicolors basically allow neovim to use more color (24-bit) instead of 256 colors (8-bit)

    updatetime configure the length of time neovim will wait before triggering plugins which is useful for some plugins, the default value is 4000 ms which is too long and causes delay.

    There are a lot more configurations we can do which we can find in the option list [here](https://neovim.io/doc/user/quickref.html#option-list)

### lazy.nvim
[lazy.nvim](https://github.com/folke/lazy.nvim) is the most stable and maintaned plugin manager for neovim. Besides being fast and pretty, it is also easy to use.

1. Write lines below into `manager.lua` file 
    ```lua
    local lazypath = vim.fn.stdpath("data") .. "/lazy/lazy.nvim"
    if not (vim.uv or vim.loop).fs_stat(lazypath) then
      local lazyrepo = "https://github.com/folke/lazy.nvim.git"
      local out = vim.fn.system({ "git", "clone", "--filter=blob:none", "--branch=stable", lazyrepo, lazypath })
      if vim.v.shell_error ~= 0 then
        vim.api.nvim_echo({
          { "Failed to clone lazy.nvim:\n", "ErrorMsg" },
          { out, "WarningMsg" },
          { "\nPress any key to exit..." },
        }, true, {})
        vim.fn.getchar()
        os.exit(1)
      end
    end
    vim.opt.rtp:prepend(lazypath)

    require("lazy").setup("plugins")
    ```
2. Write `return {}` in the `plugins.lua` file

and that's it! lazy.nvim is installed on the neovim. If we want to install any plugin, we can add the plugin inside the table `{}` in `plugins.lua` file. For example, to install plugin `Comment.nvim`, the `plugins.lua` file's content will be

```lua
return {
  {
		'numToStr/Comment.nvim',
		config = function()
			require('Comment').setup()
		end
	},
}
```

The plugin will be automatically installed next time we open neovim or we can run a command `:Lazy install` to immediately install the plugin. `Comment.nvim` plugin is a plugin that allow us to comment lines of code with a keybind (`gcc` by default). We can check if the plugin is working by trying to comment some line with the keybind.

