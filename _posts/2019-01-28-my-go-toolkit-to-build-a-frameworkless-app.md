---
title: My Go Toolkit to Build a Frameworkless App
published: true
description: How I manage to build a frameworkless app with Golang
tags: go, tools, githunt, framework
cover_image: https://thepracticaldev.s3.amazonaws.com/i/sp6baiex1tjq736iz7lk.jpg
---

A few months ago, I started [my journey from PHP to Go]({{ site.baseurl }}{% post_url 2018-09-20-my-journey-from-php-to-go %}) on a new app in my company.

Some 3 months after, I posted another Article on DEV talking about [my feedbacks on the language]({{ site.baseurl }}{% post_url 2018-12-22-its-been-three-months-since-my-first-go-loc %}).

Now it's the time to share about the libraries & tools I've been using so far. Note that I don't use any framework nor any ORM. Just the right libraries to do the job.

---

## Libraries

### 1. Viper

![viper](https://cloud.githubusercontent.com/assets/173412/10886745/998df88a-8151-11e5-9448-4736db51020d.png){:width="300px"}

🙋 **Purpose:** *<br/>
🌠 7,353<br/>
 [spf13/viper](https://github.com/spf13/viper)

### 2. Gin Gonic

![gin-gonic](https://raw.githubusercontent.com/gin-gonic/logo/master/color.png){:width="150px"}

🙋 **Purpose:** *Gin is a HTTP web framework written in Go (Golang). It features a Martini-like API with much better performance -- up to 40 times faster. If you need smashing performance, get yourself some Gin.*<br/>
🌠 *23,913<br/>*
 [gin-gonic/gin](https://github.com/gin-gonic/gin)

### 3. Mgo

🙋 **Purpose:** *A MongoDB driver for Go*<br/>
🌠 *1,385*<br/>
 [globalsign/mgo](https://github.com/globalsign/mgo)

### 4. Zap

🙋 **Purpose:** *Blazing fast, structured, leveled logging in Go*<br/>
🌠 *5,901*<br/>
 [uber-go/zap](https://github.com/uber-go/zap)

### 5. ~~Go.uuid~~ gofrs/uuid

🙋 **Purpose:** *A safe UUID package originally forked from github.com/satori/go.uuid*<br/>
🌠 *394*<br/>
 [gofrs/uuid](https://github.com/gofrs/uuid)

### 6. JWT-Go

🙋 **Purpose:** *Golang implementation of JSON Web Tokens (JWT)*<br/>
🌠 *4,849*<br/>
 [dgrijalva/jwt-go](https://github.com/dgrijalva/jwt-go)

### 7. Testify

🙋 **Purpose:** *Go code (golang) set of packages that provide many tools for testifying that your code will behave as you intend*<br/>
🌠 *6,634*<br/>
 [stretchr/testify](https://github.com/stretchr/testify)

### 8. Go-yaml

🙋 **Purpose:** *YAML support for the Go language*<br/>
🌠 *2,692*<br/>
 [go-yaml/yaml](https://github.com/go-yaml/yaml)

---

## Tools

### 1. Gomock + mockgen

🙋 **Purpose:** *Gomock is a testing library that allows you to mock your dependencies and to make assertions on them. Mockgen is a CLI tool packaged with gomock to create your mocks.*<br/>
🌠 *2,023*<br/>
 [golang/mock](https://github.com/golang/mock)

### 2. Modd

🙋 **Purpose:** *A file watcher allowing you to restart your app, to run your unit tests and much more*<br/>
🌠 *977*<br/>
 [cortesi/modd](https://github.com/cortesi/modd)

---

I hope that you'll find some of these libs/tools useful for your needs.

Don't hesitate to propose the ones you find great.

Thank you for reading!
