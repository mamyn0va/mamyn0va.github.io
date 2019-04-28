---
title: Building a Custom IDE with Tmux
published: true
description: Building a Custom IDE with Tmux
tags: cli, showdev, craftsmanship, githunt
cover_image: https://thepracticaldev.s3.amazonaws.com/i/veaudjcdm0zytasomxpn.png
---

## <i class="fas fa-terminal"></i> Hi!

Today, I want to share about a tool I've been using for a few months and which helps me a lot in my day to day job: [tmux](https://github.com/tmux/tmux/wiki).

As a software craftsman and [CLI lover]({{ site.baseurl }}{% post_url 2018-08-16-cli-love-inside %}), I'm always looking for the best tools to be as productive as possible.

Let me show you how I use *tmux* as the foundation of what I call *my custom IDE*.

## <i class="fas fa-terminal"></i> Why Tmux?

> Tmux is a wonderful terminal multiplexer that comes with some plugins.

You might wonder why you would need to use such a thing. Many terminal emulators come with built-in feature for splitting your term. 
In fact, it comes that *tmux* is much more powerful for splitting and resizing windows. 
The other reason is that *tmux* can work with any term. So if you change your term, you don't need to reconfigure all your presets. *Tmux* does it for you. 
And you can also use *tmux* without X, in a simple `tty`.

### <i class="fas fa-terminal"></i> Install it

Install *tmux* through your favorite package manager, or compile it from the sources:

```shell
$ git clone https://github.com/tmux/tmux.git
$ cd tmux
$ sh autogen.sh
$ ./configure && make
```

### <i class="fas fa-terminal"></i> Configure it

*Tmux* may look a bit raw and difficult to handle at the beginning. Fortunately, it comes with a great community that helps a lot to set it up. After trying many configurations, I've ended up with the one of [gpakosz](https://github.com/gpakosz/.tmux):

![tmux](https://cloud.githubusercontent.com/assets/553208/19740585/85596a5a-9bbf-11e6-8aa1-7c8d9829c008.gif)

It comes with presets for bindings and a great [powerline](https://github.com/powerline/powerline) look for the status bar.

Just have a loot at the [README](https://github.com/gpakosz/.tmux/blob/master/README.md) to customize it to your needs!

Personally, I find it useless to have the uptime in my status bar, so I removed it from the configuration. I also added some plugins:
- [tmux-plugins/tpm](https://github.com/tmux-plugins/tpm) *The Tmux Plugin Manager*
- [tmux-plugins/tmux-sensible](https://github.com/tmux-plugins/tmux-sensible) *Basic settings*
- [tmux-plugins/tmux-yank](https://github.com/tmux-plugins/tmux-yank) *Allows copying to system clipboard*
- [tmux-plugins/tmux-open](https://github.com/tmux-plugins/tmux-open) *Key bindings for quick opening of a highlighted file or url*
- [chriszarate/tmux-tasks](https://github.com/chriszarate/tmux-tasks) *Display the count of (urgent) tasks in the status bar. Requires [taskwarrior](https://github.com/GothenburgBitFactory/taskwarrior).*

### <i class="fas fa-terminal"></i> Use it

You have to set *tmux* as your default shell in your preferred terminal emulator. Once done, *tmux* will be launched each time you open a new term.

Basically, when *tmux* starts, it creates an empty session with a single window containing a blank pane and a status bar. The status bar is divided in 3 parts: left, middle and right. If, like me, you choose to use the configuration above, the left part will display the **session**'s name (or index if name is unset) and the uptime. The right part will display the battery, the date, the username and the hostname. In the middle, you'll see the **windows**' titles (or the name of the process running in the focused pane if the name is unset).

The basics are **sessions**, **windows** and **panes**. Each time you open a term (or a new tab in the term), *tmux* launches a new session. A new session contains a window. A new window contains a pane in your `$HOME` dir.

Bindings in *tmux* are essential: you can access them by using the prefix `Ctrl-a` or `Ctrl-b`.

Here are the main bindings I use:
- `<prefix> Ctrl-c` creates a new session
- `<prefix> c` creates a new window
- `<prefix> %` splits the current pane vertically
- `<prefix> "` splits the current pane horizontally

Now, if you feel uncomfortable with bindings, *tmux* has a great mouse mode, allowing you to select, switch and resize panes.

### <i class="fas fa-terminal"></i> Get the most of it

The killer feature of *tmux* is the **sessions**. Imagine you want to build a window with 3 panes for editing a file, play with git and run your tests. Just create a new session, split the panes and adjust their size to your needs.

*Great? Not that much.*

As there is no built-in mechanism to save session, I use the [tmuxp](https://tmuxp.git-pull.com/en/latest/) tool. This is a must-have session manager for *tmux*.

Install it through your distro's manager or:

```shell
$ pip install --user tmuxp
```

In two words: *tmuxp* allows you to easily create sessions from `yaml`/`json` files and to load 'em when you need it. And it's not only about creating the windows & panes layout: you can also run commands in each of the panes. So you can run `vim` in the main pane, `cd` to your project directory in another pane to play with `git`, and run a file-watcher in a 3rd pane to trigger your tests:

```yaml
# my-project.yaml
session_name: my project
windows:
- window_name: my custom IDE
  layout: main-vertical
  shell_command_before:
    - pj my-project
  panes:
    - vim
    - git status
    - phpunit-watcher watch
```

Then just run `tmuxp load -y my-project.yaml`.

And if, like me, you need to build up a full environment to work on your app, know that there is no limit. The session I use for my current project has many windows:
- 1 **IDE** window: 3-panes with `vim`, a term to play with `git` and a term to run the tests,
- 9 windows, one for each log file I need to grep with [jq](https://stedolan.github.io/jq/) or [lnav](http://lnav.org/),
- a window to serve the assets of my front,
- a window to run my stubs (typically a SpringBoot app).

---

**That's it!**

I hope you learned something by reading this article and that you'll have a look at *tmux*. I'm not an expert of it. I must say I don't use 10% of the features, but this tool has become an essential part of my dev env. I guess it could do the same for you.

Thanks for reading.

Bye.
