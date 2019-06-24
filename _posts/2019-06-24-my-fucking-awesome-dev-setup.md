---
title: 🔥 🤩 My Fucking Awesome Dev Setup 🤩 🔥
published: true
description: Increase your productivity with awesome Linux tools!
tags: linux, manjaro, deepin, productivity
cover_image: https://thepracticaldev.s3.amazonaws.com/i/7p67tzw49lrkyplbpbk6.png
---

> This article was inspired by this other post I saw recently: [My beautiful Linux development environment](https://dev.to/deepu105/my-beautiful-linux-development-environment-2afc).
> It always amazes me that such weekly articles always get so popular. So I wrote another one.

# 🔍 Quick Overview

- OS: [Manjaro](https://manjaro.org/)
- Windows Management: [Deepin Desktop Environment](https://www.deepin.org/en/dde/)
- IDE: [Intellij](https://www.jetbrains.com/idea)
- Editor: [Neovim](https://neovim.io/)
- Terminal: [Guake](http://guake-project.org/)
- Shell: [Fish](https://fishshell.com/)
- Tools:
  - Terminal Multiplexer: [Tmux](https://github.com/tmux/tmux/wiki)
  - Universal Launcher: [Albert](https://github.com/albertlauncher/albert)
  - File Watcher: [Modd](https://github.com/cortesi/modd)

---

# 🐧 Manjaro

[Manjaro](https://manjaro.org/) is a Linux distribution based on ArchLinux. It's a rolling release, which means that you get updates for your apps very quickly. It can be great for developers, because we don't want to wait to have security or featured updates, but it's also dangerous because such distro can break more easily. So install it at your own risk! That being said, I must tell you that I'm not a great expert of Linux, and I never got stuck with my Manjaro.

Pacman is the default CLI package manager. It's very basic and rudimentary. Now I use [yay](https://github.com/Jguer/yay) which is more user-friendly. If you prefer graphical apps, have a look at [pamac](https://gitlab.manjaro.org/applications/pamac).

# 🗔 Deepin Desktop Environment

When it comes to choosing the right desktop environment for a Linux distro, we're often forced to select the less horrible one. You may think I'm a bit excessive, but many well-known window managers (Gnome, KDE, Cinnamon, xfce, ...) are, in their default configuration, just awful.

After having tried many of them (xfce, Gnome, Budgie, i3wm, awesomewm), I finally opted for [Deepin Desktop Environment](https://www.deepin.org/en/dde/). It works like a charm out-of-the-box and as I'm a lazy developer, it was just perfect for me. The _look and feel_ is clearly inspired by macOS. The dock is very similar, the settings panel also. It's both pure and elegant. Its main advantage is also its main drawback: it's quite not configurable at all. So either it fits your need, either you choose another one.

**Deepin Apps Browser**

![](https://thepracticaldev.s3.amazonaws.com/i/tsrqfhflsv21o0rpmpfo.png)

**Deepin Settings Panel**

![](https://thepracticaldev.s3.amazonaws.com/i/hwfanqfvhlshro2dety3.png)

**Deepin Quick Launcher**

![](https://thepracticaldev.s3.amazonaws.com/i/ql551wrbi3eu5846bknb.png)

**Deepin File Manager**

![](https://thepracticaldev.s3.amazonaws.com/i/6xhvyyuw3nsqi6gcgm2v.png)

# Albert

[Albert](https://github.com/albertlauncher/albert) is an [Alfred](https://www.alfredapp.com/)-like launcher that brings you many features in a simple search bar. Just invoke it by some user-defined hotkey (I use `<ctrl>+SPC`) and start typing!

![](https://thepracticaldev.s3.amazonaws.com/i/xjsloi0sk7ir9745evv4.png)

You can trigger web searches, in-line translations, calculations, shell commands, ... and it's widely extendable with plugins.

# 👨🏼‍💻 Intellij Ultimate + Go plugin

As a Go developer, I am very pragmatic. Thus, I chose the best tool for my needs, which is Intellij with Go plugin. It has all I need in a modern IDE: completion, browsing, debugging, refactoring, syntax highlighting, ... But it's not a definitive choice. I already tried, with more or less success, several other IDE: VSCode, Atom, Vim. While the first two didn't fully work out-of-the-box, the last (Vim) was pretty impressive (with [SpaceVim](https://spacevim.org/)). But as I wasn't very comfortable with keyboard shortcuts, I finally dropped it for Intellij. I may reconsider this very soon...

# 👽 Guake + Tmux + Fish + Vim + Modd

Under Linux, you need a shell to run commands. But this shell can't run alone. It needs a terminal emulator. So imagine you want to edit a file, you'll need to run a terminal emulator (say `xterm`), that will launch your default shell (say `bash`), and then you'll be able to run `vim`. It's like russian dolls (xterm > bash > vim). And if you want to split your terminal in several panes, you'll need another layer between your terminal emulator and your shell: `tmux` (xterm > tmux > bash > vim).

🧙‍♂️ It's the magic of Unix interoperability: each app does its job well, and only its job.

## 💻 Guake

[Guake](http://guake-project.org/) is a drop-down terminal inspired by the terminal used in the game Quake. I customized it to remove scrollbars, tab bar and title bar so that it just looks like a nude terminal. By default it's hidden and it appears when I press `<F12>`. I go full screen with `<F11>`.

My conf:

- font: [Monaco for Powerline Regular, size 10](https://github.com/cstrap/monaco-font)
- transparency: 10%
- default interpreter: tmux
- theme: molokai

## 🤖 Tmux

[Tmux](https://github.com/tmux/tmux/wiki) is a powerful terminal multiplexer.

![](https://thepracticaldev.s3.amazonaws.com/i/4le028175ojug8r19zhi.gif)

> screenshot by [@gpakozs](https://github.com/gpakosz/.tmux)

I posted an article about my custom setup earlier this year. Have a look on it if you're interested: [Building a Custom IDE with Tmux]({{ site.baseurl }}{% post_url 2019-02-05-building-a-custom-ide-with-tmux %}).

## 🐟 Fish shell

[Fish](https://fishshell.com/) is a user-oriented shell with powerful features like auto-suggestion, completion, command colors, ...

As the shell is the place I spend most time in, I need to have the most useful and clear information in it. That's why I use [SpaceFish](https://github.com/matchai/spacefish) prompt. It empowers you with git information, version of your favorite language, Docker's version, Vi mode, last command status & duration, ...

![](https://thepracticaldev.s3.amazonaws.com/i/76o6vtbmlt9l2mbpqepn.gif)

I also use [oh-my-fish](https://github.com/oh-my-fish/oh-my-fish) framework to extend the shell with plugins (I recommend `grc`, `g2` `fzf`, `pj` & `z`).

## 📝 Neovim

[Neovim](https://github.com/neovim/neovim) is a refactor of Vim that brings a better plugins system and that is easier to contribute to.

[SpaceVim](https://github.com/SpaceVim/SpaceVim) is a Vim distribution with some default configuration for developers. It comes with an outline (press `<F2>`), a tree view (press `<F3>`), and many supported languages (`golang`, `php`, `python`, `javascript`, ...) for IDE features (completion, syntax highlighting, refactoring, code browsing, debugging, ...). The configuration is made easy with a simple TOML file. It's a good way to step into Vim for newbies.

![](https://thepracticaldev.s3.amazonaws.com/i/mzgw1mwazw7p6aw7wq4m.png)

## 🕺🏼 All together

Below is a tree-pane view with the following panes:
- Vim in the main pane (`golang` SpaceVim layer, with `Tagbar` and `Nerdtree` plugins)
- `modd` in the bottom-left pane to run my unit tests on modifications
- a shell in the bottom-right pane to run git commands (`git lg`)

![](https://thepracticaldev.s3.amazonaws.com/i/zza55ea1seop09ca2fqx.png)

# 👾 List of awesome dev CLI tools

- [dockly](https://github.com/lirantal/dockly): Docker UI in CLI
- [httpie](https://httpie.org/): awesome CLI HTTP client
- [jq](https://stedolan.github.io/jq/) & [fx](http://fx.wtf/): CLI JSON viewers
- [lnav](http://lnav.org/): a log file navigator
- [glances](https://github.com/nicolargo/glances): an eye on your system
- [bat](https://github.com/sharkdp/bat): cat clone with syntax highlighting and Git integration
- [exa](https://the.exa.website/): the ultimate `ls`
- [tig](https://jonas.github.io/tig/): a powerful git wrapper for CLI users
- [newman](https://github.com/postmanlabs/newman): automate your Postman tests in CLI or CI/CD
- [icdiff](https://www.jefftk.com/icdiff): a user-friendly diff tool (to use with git)

---

Here it is!

I hope you found it useful.
Don't hesitate to suggest other tools in comments!
Don't hesitate to say if you're tired of seeing such _"awesome"_ articles! ;)

