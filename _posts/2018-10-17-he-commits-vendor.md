---
title: He Commits Vendor! 😱
published: true
description: Discussion about vendoring. What are pros and cons?
tags: discuss, git, devtips, bestpractices 
---

Yesterday we had an interesting discussion between some developers of different teams in my company. The subject was: *« what is the point of vendoring? »*.

> Vendoring means committing your project's dependencies along with your code.

What happened is that some developers (including myself) discovered that some other developers commit the vendor folder in the git repository. My first reaction was pretty pejorative as I thought it was a dirty practice from another time, when we had no dependency manager. The devs explained us that it has many benefits:

- first it allows to build your app much more faster in your CI
- then it ensures you have the exact version of your dependencies
- then there is no way one of them get injected by some malware dependency
- finally you are not dependent of the network (or of the remote dependency repositories) during the build

None of these arguments satisfied me, not that they're not true, but I think each of them can be solved in a cleaner way, for example by using a cache, a custom registry with audited dependencies, and by solving directly the network issues.

And you, what do you think?
