---
layout: post
title: CLI Love Inside ❤️
date: 2018-08-16
published: true
description: How to use your terminal, shell and some awesome CLI apps to enhance your productivity?
tags: terminal, shell, cli, fish
cover_image: https://thepracticaldev.s3.amazonaws.com/i/ev7k947sk6c88zfxji5k.png
---

> The command line is a powerful tool, and sadly forsaken by most of us.

> The goal of this post is to reconcile some of you with the CLI (*Command Line Interface*), wether you are a developer or not.

When coding, we find ourselves confronted to this choice: IDE or text editor + CLI?

Modern IDE embed (almost) all the required dev tools: code edition, syntax highlighting, automatic formatting, versioning, compilation, debugging, ... and even runtime environments!

Thus, why choose a *simple* text editor?

Personally, I made this choice because I'd rather use one tool for each task than one tool for every tasks.

I prefer learn to master each tool, probably more deeply than in an IDE, even if it requires more time, than let the IDE do all the stuff for me and be stuck in case of problem.

It may seem philosophic, but in an IDE, we may sometimes feel constrained and restricted to the proposed features whereas in the CLI, there are plenty of tools, scripts, frameworks for many use cases, many environments, many languages... And the power of Unix allows the interoperability of all these commands.

Of course, if you're under Windows, you'll get into some troubles, because terminal emulators have their limits, but if you're convinced by the CLI, you'll take the plunge by switching to Linux (or maybe just use WSL if you're under Windows 10 :)).


## <i class="fas fa-terminal"></i> fish shell 🐟


[fish shell (or "fish")](https://fishshell.com/) is a shell oriented to the user interaction, in opposition to **Bash** which is more scripting-oriented. Thus, it's a good candidate for a daily and interactive usage.

It includes native syntax highlighting for many commands and tools, and autocompletion is, as well native.

**Zsh** is another credible alternative for this kind of use-cases.

Here is an example of autocompletion with **git**:

![autocompletion](http://www.imgurupload.com/images/2018/08/15/fish_autocompletione460c49118e7ac47.gif)

By typing `git`, then `<tab>`, **fish** propose the list of the **git** commands (`checkout`, `commit`, `log`, ...). By pressing several times `<tab>`, we can browse the commands until we reach the desired one, and then we just have to click on `<enter>` to validate (e.g. `git checkout`), and here **fish** shows all its magic: it's able to autocomplete the list of the git branches of your repo! Of course, it works with many other tools than git.

Two frameworks allow to enhance the fish features: [oh-my-fish](https://github.com/oh-my-fish/oh-my-fish) and [fisherman](https://github.com/fisherman/fisherman).

Both of them allow to install themes for the prompt and plugins.


### <i class="fas fa-terminal"></i> prompt 💲


At first sight, customize his prompt may seem useless, but if the CLI is your main UI, then it becomes mandatory.

It allows to know, among others, for **git**:

- on which branch you are
- if you have commits to push/pull to/from the remote
- if your index is clean or not, if you have uncommitted or untracked files
- ...

But also:

- in which folder you are
- what is the status of the last command
- the response time of the last command
- ...

There are dozens of prompts available, so everyone will be able to choose according to its own preferences. On my side, I selected two of them: [bobthefish](https://github.com/oh-my-fish/theme-bobthefish) and [neolambda](https://github.com/ipatch/theme-neolambda)​, that you can install with **oh-my-fish** : `omf install bobthefish`.

The first one, highly visual, is based on [powerline](https://github.com/powerline/powerline), a status-line for **Vim** that includes many patterns and symbols to make it more *user-friendly*:

![bobthefish](https://thepracticaldev.s3.amazonaws.com/i/hbszkvtuiywwt2ofu1e1.png)

The second one is sleeker and with less features, but yet interesting (`omf install neolambda`):

![neolambda](https://thepracticaldev.s3.amazonaws.com/i/e7lg5uvjqpt11jqbkke9.png)

Apart from the prompt, many plugins allow to enhance the user journey, in particular:

- **colorman** which adds syntax highlighting for the man pages  (`omf install colorman`)

    ![colorman](https://thepracticaldev.s3.amazonaws.com/i/n2mmy1y4saihsqr63uo9.png)

- [grc](https://github.com/oh-my-fish/plugin-grc) that adds syntax highlighting for many Unix commands: **tail**, **ping**, **cat**, **ps**, **df**, ... (to install it: `fisher grc`)
  - Example with **ping** :

    ![grc_ping](https://thepracticaldev.s3.amazonaws.com/i/61s797gefnhew4aljs4p.png)

- [pj](https://github.com/oh-my-fish/plugin-pj) is a plugin to quickly switch from a project to another, whether it is in your terminal or in your editor (`omf install pj`)
- [g2](https://github.com/fisherman/g2) is a wrapper to simplify the **git** usage.


### <i class="fas fa-terminal"></i> pimp my terminal! 💄


- [colorls](https://github.com/athityakumar/colorls) (`gem install colorls`) -- This `ls` wrapper really is a *must have*. It colors **stdout**; it uses colors intensity to emphasize the modification date of the current directory's files; it makes the file sizes *human readable*; and, on top of that, it displays the **git** status of current files/folders!

    ![colorls](https://thepracticaldev.s3.amazonaws.com/i/r4vltr66ur6uvt61a6oo.png)


### <i class="fas fa-terminal"></i> color my logs! 🌈


If you, like me, are a developer or a devops engineer, visualizing logs is thus a recurring task of your job and it becomes mandatory to have the good tools to be productive.

Modern IDE are not suited for logs viewing because they're already overloaded by your source files, and moreover, their huge weight may have a significant impact on your editor's performance. That's what happened for me in **Atom** as soon as the file's weight exceeded 10Mb.

The solution: use your terminal to tail the logs, while benefiting from available tools to autoformat them, to enable the syntax highlighting and to perform searches.

I use two different tools depending on the log type:

- **ccze** for traditional logs (**Apache**, **syslog**, **php**, ...)

    ![ccze](https://thepracticaldev.s3.amazonaws.com/i/s0sm668b1y0cjvwki27m.png)

- [jq](https://stedolan.github.io/jq/) for JSON logs

    ![jq](https://thepracticaldev.s3.amazonaws.com/i/8xusizqcb3y2ozqicsx9.png)

The benefit of **jq** is that on top of the JSON syntax highlighting, it automatically formats your logs to facilitate the reading. Thus, if you have one-liner compacted JSON logs for your ELK or any other data analysis stack, it will allow you to *unpack* your logs and to make them *human readable*.

**jq** is a much more powerful tool that would deserve its own article, as it is in fact a JSON parser with its own query description language, in the same way as **xpath** for XML, but with the simplicity of JSON.

Thus, by tailing each of your log files with `tail -f` in a dedicated term, and by piping stdout to **jq** or **ccze** depending on the type, you'll have a quick access to the information you need, formatted in an elegant way.


## <i class="fas fa-terminal"></i> other awesome CLI tools 👾

-    [ccat](https://github.com/jingweno/ccat) : syntax highlighting for **cat**
-    [tig](https://github.com/jonas/tig) : allows to enhance the ouput of many known **git** commands (e.g. `git log | tig`)
-    [howdoi](https://github.com/gleitz/howdoi) : if you're wondering how to format a date in PHP, then just type `howdoi format date php`
-    [htop](https://hisham.hm/htop/) : to display the list of current processes
-    [glances](https://github.com/nicolargo/glances) (`pip install glances`) : a supervision console for your computer (processes, RAM, network, I/O, captors, ...)
-    [clog](https://github.com/clog-tool/clog-cli) (`cargo install clog`) : generate CHANGELOGs from your **git** repo's metadata
-    [googler](https://github.com/jarun/googler) : Google CLI
-    [slacker](https://github.com/TidalLabs/Slacker) / [matterhorn](https://github.com/matterhorn-chat/matterhorn) : CLI for (respectively) **Slack** and **Mattermost**
-    [toot](https://github.com/ihabunek/toot) (`pip install toot`) : CLI for **mastodon**
-    [dockly](https://github.com/lirantal/dockly) (`npm install -g dockly`) : monitor your containers and **Docker** images from your term
-    [wunderline](http://wayneashleyberry.github.io/wunderline/) (`npm install -g wunderline`) : CLI for **Wunderlist**
-    [newman](https://github.com/postmanlabs/newman) (`npm install -g newman`) : you want to integrate **Postman** in your CI/CD pipeline? then **newman** is made for you!
- [ttyrec](https://github.com/mjording/ttyrec)/[ttygif](https://github.com/icholy/ttygif) : allow to create animated GIF from a shell session to be included in a blog post for example (that's what I used for this article)

> For each of the commands/tools quoted above, I put in brackets the command to install it. When there isn't, you'll find it in your package manager or easily on Internet.

> Most of these commands require a third-party package manager like **pip** (python), **npm** (Node.js), **gem** (Ruby) or **cargo** (Rust).

Please feel free to share your own CLI by commenting this article!