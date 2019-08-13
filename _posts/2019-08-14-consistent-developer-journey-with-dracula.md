---
title: Consistent Developer Journey with Dracula 🧛
published: true
description: How I stylized all my apps with Dracula theme to have a consistent developer journey
tags: webdev, productivity, dracula, theme, tmux, vim
cover_image: https://thepracticaldev.s3.amazonaws.com/i/cn3qgitf5xx7kr223adt.png
---

Recently, I've customized all my favorite apps with [Dracula theme](https://draculatheme.com). As the Dracula team does a great job, I quickly found themes for the following apps:
- VSCode
- Intellij
- Vim

There are also many Dracula themes for Firefox. I choose [Klorax' one](https://addons.mozilla.org/en-US/firefox/addon/klorax-dracula/).

Then, I wanted to go further and have the same journey in my terminal emulator. Fortunately, the [deepin-terminal](https://github.com/linuxdeepin/deepin-terminal) comes with a lot of bundled themes, including Dracula.

But as a Tmux user, I was a bit frustrated as the status line I used had nothing to do with Dracula. And there was no Dracula theme for Tmux. So I created one:

- [mamyn0va/tmux-dracula](https://github.com/mamyn0va/tmux-dracula)

In fact, I just forked the awesome Tmux conf from [gpakosz](https://github.com/gpakosz/.tmux) and I changed the colors according to the [color palette of Dracula](https://github.com/dracula/dracula-theme/#color-palette).

## Et voilà!

![screenshot](https://thepracticaldev.s3.amazonaws.com/i/cn3qgitf5xx7kr223adt.png)

> The shell in the screenshot is [fish](https://fishshell.com/) with [SpaceFish](https://github.com/matchai/spacefish) prompt.

> If you're looking to have the same look and feel when listing your folders (`ls`, `ll` or `lla`), I created a [Dracula theme for colorls](https://github.com/mamyn0va/dotfiles-home/blob/master/.config/colorls/dark_colors.yaml) which is a drop-in replacement for `ls` unix command.

And you're done!
