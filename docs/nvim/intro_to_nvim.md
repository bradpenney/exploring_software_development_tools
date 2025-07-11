# What is NeoVim?

[NeoVim](https://neovim.io/) (NVIM) is an Integrated Development Environment
(IDE). In software development, there are endless debates about which IDE is
best - some prefer tools like VSCode, or IntelliJ's suite of tools. It seems
like the choices are endless. However, I've committed to using NVIM as my
primary IDE (for the foreseeable future at least 😜).

## Why NeoVim?

The magic of NVIM is that it is modal - meaning that depending on the mode
you are in, the keys you press will do different things. For example, in
Command mode, presssing `D` will delete the current line, while in Insert mode,
it will insert the letter `D` at the current cursor position. So switching
modes (with `Esc` by default) gives a second layer of functionality to your
keyboard which pretty much equates to superpowers without lifting your
fingers from the keyboard.

NVIM is a fork of and inherits its magic from Vim, which has been around
since the 1990s. NVIM builds on Vim's incredible foundation but has better
performance and extensibility. One of the drawbacks of Vim is that it uses
VimScript, a Domain Specific Language (DSL) that isn't as powerful or flexible
as other programming languages. While NVIM still supports VimScript, it prefers
using Lua, a complete and powerful programming language that is easy to learn
and use.

## Which NeoVim Should I Use?

There are many different distributions of NVIM, each with its own set of
plugins and configurations. A popular way to get started with NVIM is to use
TJ DeVries' NVIM configuration, [Kickstart.nvim](https://github.com/nvim-lua/kickstart.nvim).

For myself, I have been using [LazyVim](https://www.lazyvim.org/), an
opinionated and complete NVIM setup out-of-the-box. It includes a wide range
of plugins and configurations designed to be easy and extensible.

Here's a screenshot of this file being created in LazyVim:

![Writing in LazyVim](../../images/lazyvim.png)

???+ warning "Learning Curve"

    While NVIM is a powerful tool, it does have a steep learning curve. It
    requires a different mindset and approach to text editing compared to
    traditional IDEs. The modal nature of NVIM can be challenging for new
    users, but with practice, it becomes second nature.
