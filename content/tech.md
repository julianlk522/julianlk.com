+++
title = 'Thoughts on Tech That I Have Used'
date = 2024-08-31T23:34:03-04:00
draft = true
+++

There is still a lot I don't know about these. If I get something wrong, please [let me know](../contact/)!

## Programming Languages

#### Go

Pros:

-   `defer`ing funcs is handy for cleanup
-   strong stlib
    -   `testing` lib is simple and powerful
        -   fuzz with `testing.F` and `go test -fuzz {fuzz test func}`
        -   benchmark reports with `testing.B` and `go test -bench={dir}`
        -   coverage reports with `go test -cover`
            -   see (https://tip.golang.org/doc/go1.2#cover)
-   useful / intuitive CLI
    -   `go fmt`
    -   `go test`
    -   `go run` / `go build`
-   useful concurrency abstractions:
    -   goroutines
    -   channels
-   context useful as more organized and elegant alternative to global state
-   feels like a good balance of high-level/readable and low-level/powerful

Cons:

-   drawbacks of static typing that I imagine other languages suffer from too:
    -   no union (`foo | bar`) type means either code duplication or casting to `interface{}`s which leads to other problems
        -   `interface{}` loses type-safety unless you perform runtime type assertion / reflection or if the member structs of the union type share methods that can be declared on a narrower interface, which they have not at all in my FITM use cases
    -   unmarshaling JSON is annoying
        -   requires lots of nested struct boilerplate, especially with deeply nested properties e.g., `"message":{"likes":{"user":{"id":"00941362-d9cf-4527-8f20-761f4d563da7"}}}` ...
        -   easy to mess up due to strictness on matching JSON shape to struct shape(s)
-   not easy to rename your package
    -   good excuse to ~~write a bash script~~ swipe a bash script from StackOverflow to find and replace the package name across your imports and module files
-   `if err != nil {}` everywhere
    -   but `try/catch` blocks aren't great either and they lead to more nesting, so not sure how I feel about this

#### Python

Pros:

-   delightfully readable / intuitive syntax
    -   `for name in names:`
-   huge library / tooling ecosystem

Cons:

-   no pointer variables limits low-level operations
-   lil bit slow
-   dynamic typing
    -   less safety / poor DX developing your own codebase

#### JavaScript

Pros:

-   there's a package for everything
-   DOM API is intuitive

Cons:

-   slow
-   dynamic
-   goofy
    -   `Math.max() // -Infinity`
    -   `Math.min() // Infinity`
    -   `{}+[] // 0`
    -   etc.

## Other Software

#### Wezterm (terminal emulator)

-   great documentation
-   built-in multiplexing is sweet

#### Hugo

Pros:

-   very customizable

Cons:

-   error messages are not always helpful
-   steep learning curve, I think
    -   subtle nuances that can be hard to debug
        -   `_index.md` vs `index.md`
        -   template lookup order
    -   many terms to understand
        -   template
        -   layout
        -   partial
        -   taxonomy
        -   etc.

#### Cisco PacketTracer

-   actually amazing. I would love to use this more for real network engineering.
-   intimidating UI at first glance
-   download page is somewhat hidden
