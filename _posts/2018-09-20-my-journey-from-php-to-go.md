---
title: My Journey from PHP to Go
published: true
description: One week of Go dev for a PHP dev
tags: php, go, feedback
---

As an introduction, let me tell you that I've been using Go for only one week. But I think that a newcomer's opinion can be valuable for the community and other newbies.

Let me also tell you that I'm not intending to quit PHP. It's my main language. But I need to learn a new language, to change of paradigm, and Go is the one I'm most interested in.

It's simple, close to the system, and really suited for cloud based applications. Although some frameworks exists for Web development, it's not the philosophy of Go. 

It's a language for craftsmen!

## <i class="fas fa-terminal"></i> Building my own (dis)Integrated Development Environment

As an experienced (PHP) developer, my goal is to find a way to be as productive in Go as in PHP. Thus, I'm trying to build an efficient development environment. Here are the basics:

* an editor with linting, syntax highlighting, code formatting, code browsing, an outline view, a tree view
* a term with a unit tests watcher
* a term with a running instance of my app
* a term to work with git

Regarding the editor, as an aficionado of **Atom**, I first gave it a try. The plugins **go-plus** and **ide-go** seemed interesting, but unfortunately I faced an issue that prevent me to use it.
So I tried **Intellij Ultimate** with the Go plugin, but I had a permission issue while trying to build, run or test my app.
Finally, I ended by using my good old **vim** with **vim-go** plugin. That's the only one which worked directly out-of-the-box, without any configuration.

Then, I used **tmux**+**tmuxp** to create a custom layout with vim and 3 terms for the unit tests, the running app and the git commands.

## <i class="fas fa-terminal"></i> Organizing my project

The next step is to bootstrap a project from scratch and to set the foundations for a maintainable codebase. My future app is a fairly straightforward database driven API that consumes other API. So with Go I should end up with a unique executable.

I am used to split my code as follows:
- a *controller* folder
- a *business* folder
- a *model* folder (aka *DTO*)
- a *mapper* folder (aka *DAO*)
- optionally a *common* folder for shared stuff
- an *index.php* file

For the moment, I intend to use the same organization for my Go project but I'm a bit confused by the fact that Go files are just a way to split the code but not to isolate it from the rest of the code. What I mean is that in PHP or any other OOP language, if I put a class in a file, it gets isolated from other classes (except public properties & functions). In Go, if I split my code in two files inside a given folder, it will be re-assembled by the compiler as if it was a single file. Does it mean that I need to create sub-packages for each of my files? I don't think so, but it's too early for me to answer.

## <i class="fas fa-terminal"></i> Composer VS Go modules

While PHP offers a great dependency manager ([composer](https://getcomposer.org/)) since 2012, my first impression is that it's not that simple with Go.

I heard about **godep** but it's a third-party tool and it doesn't seem to be as widely adopted as Composer for PHP (maybe I'm wrong). A colleague of mine who is highly experienced with Go told me about **Go modules**. It's still experimental in 1.11 but it should be ready in 1.12, so I decided to give it a chance.


## <i class="fas fa-terminal"></i> Paradigm shifting

I've been an OOP developer (PHP, Java) for 15 years now, so switching to a language like Go is not easy. The most difficult is to forget all about classes and objects. Instead, I can use **structs** and **interfaces**. At first it was troubling, but after few days, I'm getting more and more used to that. And it's even a good thing to get rid of constructors, getters and setters.

One other breaking change is about the asynchronous processing. In traditional PHP, all is processed synchronously. The drawback is that there is no parallelism, and that you can waste a lot of time waiting for I/O, but the advantage is that it makes your code more readable and straightforward. In Go, like in other asynchronous languages like JavaScript, you take advantage of all the resources of your host by doing certain things while some others are waiting, for example, for an API response.

Thus, I'm beginning to play with **goroutines** and **channels**...

## <i class="fas fa-terminal"></i> Picking up the right libraries

Finally, I had to find some libraries to help me with some basical stuff:
- [spf13/viper](https://github.com/spf13/viper)'s config manager
- [uber-go/zap](https://github.com/uber-go/zap)'s logger
- [gorilla/mux](https://github.com/gorilla/mux)'s router
- [satori/go.uuid](https://github.com/satori/go.uuid)'s UUID generator

---

Here it is! I'll post a new article in few months when I'll have some more experience and feedbacks to give.

Any suggestion is welcome for the newbie that I am :)

Thanks for reading.
