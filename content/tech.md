+++
title = 'Tech'
date = 2024-08-31T23:34:03-04:00
draft = false
+++

## Learning / Trying Out

-   [AutoHotkey](https://autohotkey.com/)
-   [ReaPack](https://reapack.com/)

## Considering

-   Rust
-   some functional programming language
    -   I'm intrigued by Lisp
-   maybe C ... probably not

## Opinions on Tech I Have Tried

Disclaimer: There is a lot to know about some of these, and I don't grasp anywhere near all of it. If you'd like to [point out why one of my statements is wrong](mailto:julianlindsaykaufman@gmail.com), I'd love to hear from you!

### What I have used and had an absolute joy with. No notes.

-   [**Wezterm**](https://wezterm.org/) (highly customizable terminal emulator with great documentation)
-   [**Obsidian**](https://obsidian.md/) (easily navigable, self-hosted Markdown notes)
-   [**SQLite**](https://sqlite.org/) (popular small, fast, and reliable SQL database engine)
-   [**Astro.js**](https://astro.build/) (frontend framework allowing fine blending of server-side rendering and pockets of isolated interactivity)
-   [**tmux**](https://github.com/tmux/tmux/wiki) (terminal multiplexer / macro enabler / persistent session)
-   [**fzf**](https://junegunn.github.io/fzf/) (command-line fuzzy finder)
-   [**direnv**](https://direnv.net/) (easy directory-specific, file-based loading/unloading of environment variables)
-   [**Bruno**](https://github.com/usebruno/bruno) (lightweight API testing - Postman alternative)
-   [**Cisco PacketTracer**](https://en.wikipedia.org/wiki/Packet_Tracer) (networking simulation and visualization)

### Programming Languages

#### Go

Pros:

-   strong stlib
    -   `testing` is simple and powerful
        -   fuzz with `testing.F` and `go test -fuzz {fuzz test func}`
        -   benchmark reports with `testing.B` and `go test -bench={dir}`
        -   coverage reports with `go test -cover`
            -   [https://tip.golang.org/doc/go1.2#cover](https://tip.golang.org/doc/go1.2#cover)
    -   `context`: useful, elegant alternative to global state; simple inter-process communication
    -   also `database` SQL database/driver interfaces, `image` processing, and helpful utilities via `sort`, `time`, and others
-   useful / intuitive CLI
    -   `go fmt`
    -   `go test`
    -   `go run` / `go build`
    -   I wish there were a command for renaming your packages though :(
-   concurrency primitives: goroutines and channels
-   `defer`ing is useful for cleanup sometimes
    -   Like, when you open a database connection and want to close it immediately when you done using it. You can specify that as you initiate the connection, which elegantly groups the related logic
-   really fast for being fairly high-level / readable

Cons:

-   no enum
-   static typing is annoying sometimes (? probably just skill issue):
    -   no sum type (`foo | bar`) leads to duplication or awkward interface usage to provide type safety
        -   you can cast any type to `interface{}` then later perform runtime type assertion / type switching / reflection, but it's kind of gross and excessive
        -   https://zackoverflow.dev/writing/hacking-go-to-give-it-sumtypes
    -   Strictly matching JSON to struct shape when unmarshaling requires some boilerplate, especially with deeply nested properties e.g., `"message":{"likes":{"user":{"id":"00941362-d9cf-4527-8f20-761f4d563da7"}}}` ...
-   no easy way (?) to use shared data across package test suites
    -   `TestMain` works well within one package for multi-test setup / teardown but not across packages. A shared database connection, like for integration tests, seems to require duplicating something in each package's `TestMain`.
    -   [https://stackoverflow.com/a/70385157](https://stackoverflow.com/a/70385157)
-   lots of `if err != nil { ... }`
    -   But `try/catch` blocks lead to more nesting, which hurts readability. Overall I like errors as values.

#### Python

Pros:

-   readable, intuitive `for name in names:` syntax
-   huge library / tooling ecosystem

Cons:

-   no pointer variables limits memory allocation control
-   dynamic typing
    -   less safety / poorer DX developing in a non-typed codebase
-   indentation slightly harder to read / debug than `{}` blocks
-   not super fast

#### JavaScript

Pros:

-   run natively in web browsers
-   interact with webpage [DOM](https://developer.mozilla.org/en-US/docs/Web/API/Document_Object_Model) elements
-   some elegant and handy convenience features:
    -   [destructuring](https://javascript.info/destructuring-assignment)
    -   [optional chaining](https://github.com/tc39/proposal-optional-chaining)
-   huge ecosystem

Cons:

-   goofy
    -   `Math.max() // -Infinity`
    -   `Math.min() // Infinity`
    -   `{}+[] // 0`
    -   etc.
-   same stuff as Python, minus the indentation

#### Bash

-   Pipes are awesome
-   Kind of arcane syntax, IMO
-   Haven't used it enough to have more concrete opinions

### Other Software

#### Neovim

Pros:

-   everything is configurable and extensible
-   lightweight compared to, e.g., VS Code
-   clout

Cons:

-   initial configuration is intimidating
    -   [kickstart.nvim](https://github.com/nvim-lua/kickstart.nvim) helps
-   steep learning curve
-   some helpful features are not built-in or not as intuitive to configure / use as with other IDEs
    -   project-wide find and replace
        -   can be achieved with `:cfdo %s /old/new/g | update | bd` after loading files into the quickfix list, e.g., with <c-Q> from telescope.nvim grep search
    -   integrated testing UI
        -   [https://github.com/nvim-neotest/neotest](https://github.com/nvim-neotest/neotest) exists but seems awkward to set up

#### Hugo

Pros:

-   very configurable
-   very fast builds

Cons:

-   fairly steep learning curve, in my opinion
    -   subtle nuances that can be hard to debug
        -   `_index.md` list page vs `index.md` regular page
        -   template lookup order
    -   jargon-heavy
        -   template
        -   layout
        -   partial
        -   taxonomy
        -   etc.
-   error messages are not always helpful or explicit
    -   . e.g., when I accidentally .gitignore'd `themes/lugo/layouts/_default` and ran the build, Hugo subtly outputted a one-line `WARN` log but continued spinning up the 16.67% of the site that wasn't broken (1 page / 6 had a dedicated layout living outside the theme's `_default` folder, so it could build)

#### Git

Pros:

-   rather helpful
-   generally easy and autopilot-able

Cons:

-   ugly documentation
    -   but, at least third-party documentation is good
-   when automatic merging screws up and you get to untangle a complete mess
