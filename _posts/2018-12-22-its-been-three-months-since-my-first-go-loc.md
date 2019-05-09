---
title: It's Been Three Months Since My First Go LoC 🤓 🎓 
published: true
description: What I've learned as a Go developer in the last 3 months
tags: go, learning, feedback, beginners
cover_image: https://thepracticaldev.s3.amazonaws.com/i/s284unlht6jzs5qaql3n.png
---

It's been 3 months since I wrote my first Line of Code in Golang.

See my first post [here]({{ site.baseurl }}{% post_url 2018-09-20-my-journey-from-php-to-go %}).

I'm still excited about learning this new language but it seems I faced my first issues with Go. I'll talk about it later.

I introduced one of my previous post by saying that PHP was my main language and that I won't switch definitively to Go. Maybe this could change...

I really enjoy strong typing and compilation. I can have real-time feedback on my code without running it and without additional 3rd-party tools. The Go SDK is fully featured for linting, formatting, testing, compiling, running.

# <i class="fas fa-terminal"></i> Reading from request's body

In my previous post, I was also saying that Go was a language for craftsmen. But this was far from the truth. I'll take an example: in Go, if you want to read the HTTP request's body in a middleware to analyze it for logging purpose, it becomes tricky:

```golang
func LoggingMiddleware(handler http.Handler) http.Handler {
    return http.HandlerFunc(func(w http.ResponseWriter, r *http.Request) {
        // first, read the body and put it in a variable
        body, err := ioutil.ReadAll(r.Body)
        if err != nil {
            log.Printf("Error reading body: %v", err)
            http.Error(w, "can't read body", http.StatusBadRequest)
            return
        }

        log.Printf("incoming request's body: %s", body)

        // And now, re-assign the request's body with the original data
        r.Body = ioutil.NopCloser(bytes.NewBuffer(body))

        // finally, call next handler
        handler.ServeHTTP(mrw, r)
    })
}
```

As you may guess, reading the request's body makes it empty. That's why you have to re-assign it afterwards. That's because request's body is a kind of stream you can read byte after byte. It can be useful if you want to start the request's processing without having to read all the data. But in most cases, you don't need that.

# <i class="fas fa-terminal"></i> (un)-marshalling

(un)marshalling is another thing that is not straightforward. What you need to know is that unmarshalling from JSON, BSON, YAML or whatever to a struct will almost never fail.

```golang
package main

import "encoding/json"
import "fmt"

type User struct {
	Name string `json:"name"`
	Age int `json:"age"`
}

func main() {
	var bob, alice User
	json.Unmarshal([]byte(`{"name":"bob","age":25}`), &bob)
	err := json.Unmarshal([]byte(`{"name":"alice","address":"NY"}`), &alice)
	
	fmt.Printf("err: %v\n", err)
	fmt.Printf("bob: %v\n", bob)
	fmt.Printf("alice: %v\n", alice)
}

```

Output:

```
err: <nil>
bob: {bob 25}
alice: {alice 0}
```

In this example, I omitted the `age` attribute in the JSON of Alice, but Go unmarshals it with no error. Confusing? That's because Go allocates default values for all missing attributes. In fact, you just have to provide valid JSON. 

*Note that supplementary JSON attributes are just ignored.*

What really confused me was that I expected unmarshalling to fail with missing attributes. But if you want to check what you really got in the JSON, you can't trust the unmarshalling, you have to do it by yourself by checking all the fields of your struct manually. If one of the field has the default value for its type (e.g. 0 for integers, or empty string), then it could mean that it was missing from the JSON. But it could also mean that the client sent `{"name":"alice","age":0}`. Then, how to distinguish between missing attributes and attributes with default values? You could unmarshal to a `map[string]interface{}` and check manually that no key is missing, but it would be so WTF?!

*If someone knows a better solution, don't hesitate!*

# <i class="fas fa-terminal"></i> Unit testing

Writing unit tests is the worst part of my developer journey in Golang. As an experimented OOP developer in PHP and Java, I used to write a lot of unit tests to have the best code coverage I can reach. In order to achieve it, I design my code with a high usage of dependency injection to allow me to replace dependencies by mocks in unit tests. 

With Go, there is no equivalent of PHPUnit or JUnit that allows to quickly mock any dependency. Mocking in Go is based on interfaces. If you're using a struct as a dependency somewhere in your code, you have to write a custom interface for it. That's where Go lacks of consistency. Depending on the library you're using, if you're lucky, it will provide an interface and not a struct (that's the case for [gin-gonic](https://github.com/gin-gonic/gin) & [uber-go/zap](https://github.com/uber-go/zap)). But otherwise, you may encounter the mocking hell (that's the case with [mgo mongo driver](https://github.com/globalsign/mgo)).

The drawback of writing interfaces for dependency injection is that you will lose the completion in your IDE. Let me explain it: when you write an interface for a 3rd-party library, you just put the functions you need in it. It means that if some day you want to use another function of that library, you don't even know what are the available functions. You can't hit ctrl+Space on the object to auto-complete because it will only display the functions of your custom interface, not the one of the original library object. So you have to browse the official doc on internet.

*Knowing that, I would recommend to prefer "mocking-ready" libraries. It will save your time!*

## <i class="fas fa-terminal"></i> Mocking your dependencies

Depending on your testing strategy, you may have to mock some parts of your code, or some external library. As I said earlier, the first thing is to use interfaces instead of structs in your code, wether it be in the prototypes of your functions or in a Dependency Injection Container.

The principle is really simple: you just have to switch the real object with a struct implementing the interface you want to mock. You can do it manually, or you can do it with [gomock](https://github.com/golang/mock) if you want a fully featured mock, allowing you to count the number of calls, to check the types or even to capture the arguments. The only drawback of gomock is that you have to generate the mocks manually, and to update them each time you modify the interface. You also have to commit the mocks along with your "real" code.

# <i class="fas fa-terminal"></i> My custom development environment

In my first post about Go, I was saying that I tried many IDE: Atom, Intellij Ultimate and Vim. After that, I also tried Visual Studio Code which was pretty nice, but none of them fully statisfied me. So, as many of my colleagues use Intellij, I gave it a new try and finally succeeded to make it work. I use it to write code, but also to debug and to browse the code.

Apart from Intellij, I also have a customized terminal with 3 panes using tmux. One of the panes is for playing with git or to type any other command. One is dedicated to vim. The last one is to run my app, check my conf and run the tests, all of this being possible with [modd](https://github.com/cortesi/modd), a file watcher:
- any modification of the codebase triggers a new run of my app (`go run ./...`),
- any modification of a test file triggers the tests (`go test ./...`)
- any modification of my conf files triggers a new check using [pykwalify](https://github.com/Grokzen/pykwalify) which is a JSON/YAML validator.

The 3rd pane is a Vim allowing me to quickly edit files.

If you're interested by getting into `tmux`, you can have a loot at this [other post]({{ site.baseurl }}{% post_url 2019-02-05-building-a-custom-ide-with-tmux %}).

# <i class="fas fa-terminal"></i> Conclusion

What I miss the most as a PHP developer is the community and the online resources. It seems like the official Go doc (which is great) is quite the only available resource, except posts on StackOverflow.
What is also confusing is that if you google something like "go + stuff", you won't get many results. You'll have more luck with "golang + stuff".

Anyway, these obstacles didn't hindered my motivation and my enthusiasm. I love the simplicity of the language.

I'll keep you informed of my progress in a future post.

Thanks for reading!
