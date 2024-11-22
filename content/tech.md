+++
title = 'Tech'
date = 2024-08-31T23:34:03-04:00
draft = false
+++

## Learning / Practicing / Testing Now

-   [fzf](https://junegunn.github.io/fzf/)
-   [direnv](https://direnv.net/)
-   more bash
-   more vi/Vim
-   more SQL(ite)
    -   [window functions](https://www.sqlite.org/windowfunctions.html)
-   more Git
    -   rebase, cherry-pick, bisect, etc.
-   Neovim plugin ecosystem
-   tmux
-   Hugo SSG (powering this website)

## Considering

-   Redis
-   a functional programming language
    -   tentative considerations: Elixir, Clojure, Haskell
-   [Bun](https://bun.sh/)

## Thoughts on Tech That I Have Used

There is still a lot I don't know about these. If you see something incorrect, please [let me know](mailto:julianlindsaykaufman@gmail.com).

### Programming Languages

#### Go

Pros:

-   strong stlib
    -   `testing` lib is simple and powerful
        -   fuzz with `testing.F` and `go test -fuzz {fuzz test func}`
        -   benchmark reports with `testing.B` and `go test -bench={dir}`
        -   coverage reports with `go test -cover`
            -   see [https://tip.golang.org/doc/go1.2#cover](https://tip.golang.org/doc/go1.2#cover)
-   useful / intuitive CLI
    -   `go fmt`
    -   `go test`
    -   `go run` / `go build`
-   useful concurrency primitives:
    -   goroutines
    -   channels
-   context useful as more organized and elegant alternative to global state
-   `defer`ing funcs is handy for cleanup
-   feels like a good balance of high-level/readable and low-level/powerful

Cons:

-   no convenient way (that I have yet found) to use shared data across package test suites
    -   `TestMain` works well within one package for multi-test setup / teardown but not between packages. If you want a shared database connection for integration tests, you'll need to duplicate some code to run in the `TestMain` of each related package.
    -   [https://stackoverflow.com/a/70385157](https://stackoverflow.com/a/70385157)
-   not easy to rename your package
    -   Well, it's easy enough to replace the imports / package declarations / go.mod with a script. But I think that would be a good feature for the Go CLI.
-   `if err != nil {}` everywhere
    -   But `try/catch` blocks lead to more nesting and possibly more difficulty locating error sources. Overall, I think I prefer Go's errors-as-values system.
-   drawbacks of static typing that I imagine other languages suffer from too:
    -   no sum type (`foo | bar`) makes things hard sometimes
        -   code duplication, incomplete type safety, poor ergonomics, or a combination are difficult to avoid
            -   https://zackoverflow.dev/writing/hacking-go-to-give-it-sumtypes/
        -   you can cast any type to `interface{}` and then perform runtime type assertion / type switching / reflection, but it's a bit of a rigmarole
    -   Strictly matching JSON shape to struct shape when unmarshaling can require a lot boilerplate, especially with deeply nested properties e.g., `"message":{"likes":{"user":{"id":"00941362-d9cf-4527-8f20-761f4d563da7"}}}` ...

#### Python

Pros:

-   delightfully readable / intuitive syntax
    -   `for name in names:`
-   huge library / tooling ecosystem

Cons:

-   no pointer variables limits control of memory allocation
-   dynamic typing
    -   less safety / poorer DX developing in a non-typed codebase
-   indentation slightly harder to read / debug than `{}` blocks
-   a bit slow

#### JavaScript

Pros:

-   some convenience features that are really elegant and handy:
    -   [destructuring](https://javascript.info/destructuring-assignment)
    -   [optional chaining](https://github.com/tc39/proposal-optional-chaining)
-   a package for everything
-   run natively in web browsers
-   intuitive [DOM APIs](https://en.wikipedia.org/wiki/Document_Object_Model#Manipulating_the_DOM_tree)

Cons:

-   slow
-   dynamic
-   goofy
    -   `Math.max() // -Infinity`
    -   `Math.min() // Infinity`
    -   `{}+[] // 0`
    -   etc.

#### Lua

Pros:

-   simple type system

Cons:

-   1-indexed (¬_¬")

### Other Software

#### Neovim

Pros:

-   very configurable
-   base IDE is very lightweight compared to, e.g., VS Code
-   everything Vim offers and more
    -   [LSP](../tech-qna#what-is-lsp-what-problems-does-it-solve) client
    -   lenient extension potential with Lua

Cons:

-   initial configuration is intimidating (though [kickstart.nvim](https://github.com/nvim-lua/kickstart.nvim) helps immensely)
-   some helpful features are not built-in or not as intuitive to configure / use as with other IDEs
    -   project-wide find and replace
        -   can be achieved with `:cfdo %s /old/new/g | update | bd` after loading files into the quickfix list, e.g., with <c-Q> from telescope.nvim grep search
    -   integrated testing UI
        -   [https://github.com/nvim-neotest/neotest](https://github.com/nvim-neotest/neotest)
-   unlike vi or Vim(.tiny), not included by default in many Linux distributions, so not readily available after SSHing into some unconfigured remote server
-   steep learning curve

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
    -   . e.g., when I accidentally .gitignore'd `themes/lugo/layouts/_default` and ran the build, Hugo gave a subtle one-line `WARN` log and proceeded to spin up the 16.67% working site (1 page / 6 had a dedicated layout living outside the theme's `_default` folder, so it built correctly)

#### Cisco PacketTracer

-   amazing
-   download page is somewhat hidden away

### Others I have used that are just an absolute joy to work with. No notes.

-   **Astro.js** (frontend framework allowing fine blending of server-side rendering and pockets of isolated interactivity)
-   **tmux** (terminal multiplexer / macro enabler / persistent session)
-   **Wezterm** (highly customizable terminal emulator with great documentation)
-   **Obsidian** (easily navigable Markdown notes)
-   [**Bruno**](https://github.com/usebruno/bruno) (open source, lightweight Postman alternative)
