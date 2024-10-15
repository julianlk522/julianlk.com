+++
title = 'Tech Q&A'
date = 2024-09-04T23:29:32-04:00
draft = false
+++

This page is a collection of questions that I had about various tech concepts and the best answers that I could give now, to my earlier self and to any other curious readers, to help it make sense. Topics are ones which I found especially confusing or not suitable to be answered by a quick Google search.

These writings also serve as notes to help me learn and for future reference.

More topics will be added over time.

As always, if you see something false, please [let me know](mailto:jxl1729@miami.edu). I try hard to fact-check everything but am not perfect.

## Table of Contents

{{< toc >}}

## General-Purpose Programming

#### What is the point of closures? Why not simply combine the inner and outer functions into one?

Closures allow creating hidden state inside the outer function's lexical scope (the _enclosing_ scope) which is preserved (_captured_) after the outer function finishes executing. The hidden state is accessible (i.e., can be referenced or _closed over_) in the inner function but safely separated from the rest of your code. And each call to the outer function spawns a new, distinct instance of the encapsulated state.

```
function create_counter() {
    let count = 0

    // Inner function (closure)
    function counter() {
        console.log(++count)
        return count
  }

    return counter
}
```

Distinct instances of state encapsulation for each call:

```
const counter1 = create_counter()
const counter2 = create_counter()

counter1() // 1
counter1() // 2
counter1() // 3

counter2() // 1
counter2() // 2
```

State is "safe," not directly accessible:

`console.log(counter1.count) // undefined`

A function containing no other functions _could_ contain its own state, isolated from the rest of the program, but the difference is that it cannot be invoked `n` times to produce `n` autonomous instances; it is limited to the single instance of state declared in its definition, which also cannot persist between calls.

Attempting to merge outer and inner functions...

```
function counter() {
    let count = 0
    console.log(++count)
    return count
}
```

...prevents isolated instances with preserved state:

```
const counter1 = () => counter()
const counter2 = () => counter()

counter1() // 1
counter1() // 1
counter2() // 1
counter2() // 1
```

You could store the state outside the function scope, which would allow it to persist between calls, but that would expose it outside manipulation and would require knowing the number of instances in advance.

So, closures are a way to allow dynamic regeneratation of private, preserved state.

The same effect can be achieved with a class. The private state could be represented as a property of the class and the inner function could be a method. Calling a class constuctor (e.g., `new Thing(...)`) would be equivalent to calling the outer function.

Both the [OOP](https://en.wikipedia.org/wiki/Object-oriented_programming) implementation (classes) and [FP](https://en.wikipedia.org/wiki/Functional_programming) implementation (closures) are valid, depending on the programming language and existing design patterns in the codebase, though classes do not always guarantee separation of state from other parts of the codebase.

Example with classes:

```
class Counter {
  constructor() {
    this._count = 0
  }

  increment() {
    console.log(++this._count)
    return this._count
  }
}

const counter1 = new Counter()
const counter2 = new Counter()

counter1.increment() // 1
counter1.increment() // 2
counter1.increment() // 3

counter2.increment() // 1
counter2.increment() // 2
```

Property is _not_ private in JavaScript classes:

`console.log(counter1._count) // 3`

#### What is the point of generator functions? When are they preferable to ordinary functions?

**TL;DR**:
they aren't explicitly required for anything, but they may help with organization and readability.

1.  Breaking up resource requirements for expensive tasks (i.e., [_lazy evaluation_](https://www.tutorialspoint.com/functional_programming/functional_programming_lazy_evaluation.htm)). Maybe you want to read lines from a 4GB file without loading the entire 4GB into memory, or read data coming from an infinite stream, so you create a generator function to stream line by line. Unused lines are never consumed and their memory requirements can be avoided.

    You'll need to keep track of how much of the file / stream has already been read as you continue. Managing state between calls like this is possible with closures but generators do it inherently.

With closure:

```
function lazy_sequence() {
    let i = 0;
    return function() {
        return i++
    }
}

const sequence = lazy_sequence()
console.log(sequence()) // 0
console.log(sequence()) // 1
console.log(sequence()) // 2
```

With generator:

```
function* lazy_sequence() {
    let i = 0;
    while (true) {
        yield i++
    }
}

const sequence = lazy_sequence()
console.log(sequence.next().value) // 0
console.log(sequence.next().value) // 1
console.log(sequence.next().value) // 2
```

2. Generators also integrate nicely into [iterators](https://en.wikipedia.org/wiki/Iterator) and allow usage of `for-of` loops and other constructs that can simplify readability where alternative solutions may not make that so easy.

```
async function asynchronous_task() {
    await new Promise(...)
    ...
}
```

With callback:

```
async function stream_task_results(callback, stop_condition) {
    while (true) {
        const task_results = await asynchronous_task()
        await callback(task_results)
        if (stop_condition(task_results)) {
            break
        }
    }
}

stream_task_results(
    task_results => do_something(task_results), // callback
    task_results => is_time_to_stop(task_results) // stop_condition
)
```

With generator:

```
async function* stream_task_results() {
    while (true) {
        yield await asynchronous_task()
    }
}

async function consume_stream() {
    const task_results = stream_task_results()
    for await (const tr of task_results) { // convenient iterator syntax
        do_something(tr)
        if (is_time_to_stop(tr)) {
            break
        }
    }
}

consume_stream()
```

#### Are there _exclusive_ use cases for classes compared to functions? Is there a strong reason to use them, aside from just preference or matching some existing code's design themes?

1. Classes provide distinct namespaces, functions do not
    - no mixing up `foo()` and `foo()` if they are `bar.foo()` and `baz.foo()`
2. (opinion) Classes intuitively group related data and separate unrelated data
    - see [https://www.reddit.com/r/learnpython/comments/1mc8ih/comment/cc7uyxx](https://www.reddit.com/r/learnpython/comments/1mc8ih/comment/cc7uyxx)

#### Why is an in-memory database sometimes preferred to a disk-based database?

No I/O operations required for storage and no [serialization](https://en.wikipedia.org/wiki/Serialization) required for transmission, so reads / writes are fast.

Not preferred when:

-   storage capacity is a concern and RAM is in shorter supply than disk storage
-   persistent changes required

## Tech Ecosystem

#### What is RSS all about? Should you care about it?

**RSS**: _Really Simple Syndication_

That depends on whether you follow a lot of blogs or other (relatively basic) sources that are periodically updated. RSS helps you keep up with those. You configure an _RSS aggregator_ which fetches an XML _sitemap_ file from the sites you specify. The sitemap overviews site content, and your aggregator (or _reader_) will detect and report any updates back to you in a _syndicated_ location (hence the 2nd "S").

You basically can check a single feed to get automatic updates from multiple, maybe many, sites of your choosing. But the content must be presentable in XML format, so it won't work well for more sophisticated sites or web-apps, like determining changes to the [Figma](https://www.figma.com/) engine, for example.

#### What is LSP? What problem(s) does it solve?

**_LSP_**: _Language Server Protocol_

LSP is an [open standard](https://github.com/microsoft/language-server-protocol) providing a uniform communication format for code editors and [IDEs](https://en.wikipedia.org/wiki/Integrated_development_environment) to request and receive language-specific tooling from selected dedicated intelligence servers over [JSON-RPC](https://en.wikipedia.org/wiki/JSON-RPC). Guidance might be:

-   auto complete suggestions
-   syntax highlighting
-   error checking
-   documentation on hover
-   etc.

LSP solves the problem of providing development support features for a range of languages, across a range of editors and IDEs, without duplicating implementation and without loading any such features that you don't need.

Without a common protocol, each editor would need to independently configure support for each language's unique APIs, which would be reinventing the wheel if such tooling already exists elsewhere. It allows convenient access to existing, standardized language support on an ad hoc basis.

see:

-   [https://microsoft.github.io/language-server-protocol/](https://microsoft.github.io/language-server-protocol/)
-   [https://groups.google.com/g/bbedit/c/fFu9QnJI-Tc/m/hC8rq3I5BAAJ](https://groups.google.com/g/bbedit/c/fFu9QnJI-Tc/m/hC8rq3I5BAAJ)

#### Who maintains LSP servers and their networking needs?

Code upkeep: that varies, though [Microsoft maintains a handful of core implementations](https://microsoft.github.io/language-server-protocol/implementors/servers/).

Server hosting: servers generally run locally in a background process spawned by your editor / IDE, so no there is no remote networking involved. No, the maintainers are not collecting and selling your keystrokes to the NSA.

## Web Development

#### When designing client-server web APIs, when might you want to choose REST, REST + GraphQL, or RPC? What are the unique benefits of each style?

###### [TL;DR](#with-that-said)

First, an overview of each:

###### REST

-   Created to guide reliable World Wide Web architecture<sup>[\[1\]](#www-ref)</sup>
-   Adheres to a uniform interface; the API is standardized such that any client can consume it using the same standard operations
    -   All available _resources_ (meaningful, requestable pieces application data) are:
        -   uniquely identifiable
        -   predictably modifiable through their unique identifiers
        -   fully discoverable and traversable through an initial response via hypermedia links (see [https://en.wikipedia.org/wiki/HATEOAS](https://en.wikipedia.org/wiki/HATEOAS))
            -   e.g., if the resource is a paginated list of links that don't all fit on one page, the response should indicate that there is a next page. Or if the resource is HTML, the HTML should contain links to all other reachable HTML pages.
    -   Response messages include all information needed to process them
    -   (Often achieved through a combination of HTTP method + endpoint (URI)<sup>[\[2\]](#URI-ref)</sup>)
-   Stateless; each request contains all necessary data to utilize the associated resource; there is no concept of a session
-   Responses must indicate whether they are cacheable
-   Layered system involving seamless interactions with intermediary servers
    -   Load balancing, [reverse proxy](https://stackoverflow.com/a/366212), caching, etc.
    -   _Seamless_ meaning the client should not experience altered communication results; representations of resources do not change whether coming directly from the server or from an intermediary
-   (Optionally) Server can send executable code to extend client functionality
    -   e.g., JavaScript to run on an HTML page

**In practice it is rare for all of the above guidelines to be followed strictly in an HTTP-based web API** (HTTP being the most common protocol in REST or REST-like implementations).

-   e.g., cookie-based authentication is not stateless

###### GraphQL

-   Not just a style and not autonomous: GraphQL is a request/response simplification language. It runs on the same server as an existing API to translate [declarative](https://en.wikipedia.org/wiki/Declarative_programming) query/mutation/_subscription_<sup>[\[3\]](#subscription-ref)</sup> instructions into requests to that API, consolidating its responses into unified graphs which are returned to the client.
    -   Effectively merges multiple API calls into one
    -   Removes need to keep track of numerous endpoints since all requests originate from the same one
-   Adheres to a pre-defined API schema
    -   Enforces request operation and input validity through strong data typing
        -   Helps to improve error message specificity
    -   Prevents overfetching; ensures responses contain only specifically requested data, omitting excess from the underlying API's responses
    -   Allows procedurally generating API documentation through schema [introspection](https://spec.graphql.org/draft/#sec-Introspection).

###### RPC

-   Allows direct access to calling server functions as if executed locally
    -   By default offers high level of freedom in executed actions
-   HTTP connections are not as ubiquitous as with REST, though are used in many implementations e.g., gRPC
    -   Alternative implementations may involve only requests and asynchronous handling, e.g., JSON-RPC, instead of HTTP-like synchronous request/response cycle
-   Can be stateful
-   Implementations vary and are prone to incompatibility across different client types
    -   Though may circumvent this with a standardized [Interface Description/Definition Language (IDL)](https://en.wikipedia.org/wiki/Interface_description_language) to create cross-system (language, OS) compatibility

###### With that said...

###### Which APIs might "REST" (or an approximation of it) be best suited for?

In as simple a description as possible: ones prioritizing reliability and broad, long-term compatibility.

-   Confining possible interactions through specific URI definitions
-   Clarifying availability of URIs via hypermedia links in returned responses
-   Compatibility across varying client types, especially for World Wide Web access

###### Which APIs might benefit most from GraphQL?

In as simple a description as possible: the most complex, resource-intensive ones.

-   Sprawling APIs with complex consumption patterns where the requested data must come from combinations of endpoints
-   APIs using many endpoints that are hard to keep track of
-   APIs with heavy request loads where overfetching data is a significant expense

###### Which APIs might prefer to use RPC?

In as simple a description as possible: ones responsible for private, high-trust actions with a greater priority on performance over broad support. e.g., [microservice](https://en.wikipedia.org/wiki/Microservices)-based backends.

-   Freedom to execute server actions directly can be a security vulnerability. It is less of a concern if the RPC endpoint is kept private.
-   Non-standardization has the benefit of optionally neglecting delivery of non-essential data, e.g., HTTP response status codes, which can improve processing efficiency at the expense of interoperability. This is a worthwhile trade-off if interoperability doesn't matter much.

<a name="www-ref"></a>[1]. The World Wide Web has a distinct meaning from the Internet. In short, the Internet describes the global network of computer networks which utilize packet-based routing to exhange data; the World Wide Web describes a system that indexes resources (various desired data) by Uniform Resource Identifiers (URI). See [https://www.w3.org/help/#webinternet](https://www.w3.org/help/#webinternet).

<a name="URI-ref"></a>[2]. As a side note, a URI is a unique resource identifier, and a URL is a URI that includes the resource's required access protocol. Since we know the protocol here is HTTP(S), it is most accurate to use "URI." See [https://danielmiessler.com/p/difference-between-uri-url/](https://danielmiessler.com/p/difference-between-uri-url/).

<a name="subscription-ref"></a>[3]. A subscription is a long-lived request that continues to fetch data automatically in response to each occurrence of a specified event.

see:

-   [https://cloud.google.com/blog/products/application-development/rest-vs-rpc-what-problems-are-you-trying-to-solve-with-your-apis](https://cloud.google.com/blog/products/application-development/rest-vs-rpc-what-problems-are-you-trying-to-solve-with-your-apis)
-   [https://www.baeldung.com/cs/rest-vs-http](https://www.baeldung.com/cs/rest-vs-http)
-   [https://stackoverflow.com/a/26049761](https://stackoverflow.com/a/26049761)
-   [https://www.visual-paradigm.com/guide/development/what-is-rest-api/](https://www.visual-paradigm.com/guide/development/what-is-rest-api/)
-   [https://spec.graphql.org/draft/#sec-Overview](https://spec.graphql.org/draft/#sec-Overview)
-   [https://www.howtographql.com/basics/1-graphql-is-the-better-rest/](https://www.howtographql.com/basics/1-graphql-is-the-better-rest/)

## SQL

#### If creating table indexes speeds up reads, why not always create an index for every column?

1. Consumes more RAM. Exceeding RAM limit of your computer / virtual machine means frequently swapping to / from disk storage, which is slow and possibly expensive.
2. Slows write / delete times. Each index needs to be updated along with its associated columns to stay synced.

#### Why _does_ creating a table index improve read performance? Is the table's primary key not already an index in the traditional sense?

Primary keys are sufficient as unique identifiers for each row, but they are _not alphabetically sorted_. Indexing sorts the queryable data so that a full table scan is not needed to locate a particular row. A [linear search](https://en.wikipedia.org/wiki/Linear_search) can replaced with a [binary search](https://en.wikipedia.org/wiki/Binary_search), bringing time complexity from **O(n)** down to **O(log n)**. If searching a one-million-row table, the max number of searches to find a target drops from **1,000,000** to **20**.

see [https://www.atlassian.com/data/sql/how-indexing-works#:~:text=Let's,table:](https://www.atlassian.com/data/sql/how-indexing-works#:~:text=Let's,table:)

### SQLite

#### If WAL mode is faster than using a rollback journal, involves less latency from disk I/O operations, and handles concurrent reads/writes better than other modes, why is it not always used and set by default?

[Per the creators of Django and litestream.io/](https://www.reddit.com/r/sqlite/comments/wll1nu/comment/ijx15md/?utm_source=share&utm_medium=web3x&utm_name=web3xcss&utm_term=1&utm_content=share_button), the main reason is to ensure backwards compatibility with older SQLite versions.

[A few other catches](https://sqlite.org/wal.html):

1. WAL does not support network filesystems; the database must be accessed only from a single host computer
2. WAL cannot ensure [atomicity](<https://en.wikipedia.org/wiki/Atomicity_(database_systems)>) for transactions involving multiple attached databases
3. WAL is not optimized for systems requiring mostly reads and few writes

## Linux

#### Why does Debian have both `apt` and `apt-get` commands for package management?

`apt` is newer and offers a more high-level, user-friendly interface than `apt-get`.

`apt` has a few noteworthy features that `apt-get` does not, such as automatically removing obsolete package versions with `apt upgrade` or recommending suggested package installs to fix dependency issues.

see [https://aws.amazon.com/compare/the-difference-between-apt-and-apt-get/](https://aws.amazon.com/compare/the-difference-between-apt-and-apt-get/)

#### How do `sh` and `bash` differ? How might `#!/bin/bash` vs. `#!/bin/sh` [shebangs](<https://en.wikipedia.org/wiki/Shebang_(Unix)>) differ in their effects on subsequent code?

`sh` (Shell Command Language or Bourne Shell) is defined in the [POSIX](https://stackoverflow.com/a/1780614) standard and is the foundation for multiple derived implementations, including `bash`. `bash` is a [superset](https://www.hackterms.com/superset) of `sh` that includes other commands like `history` and other features like [process substitution](https://www.linkedin.com/pulse/difference-between-sh-bash-linux-alok-mishra-soogc/#:~:text=Process%20Substitution). Scripts that use these features _may\*_ fail when told to run with `sh`.

\*In Linux distributions such as Ubuntu or Linux Mint, however, `#!/bin/sh` is a [symlink](https://www.hackterms.com/symlink) to `#!/bin/dash` ([like bash but faster](https://lwn.net/Articles/343924/#:~:text=The%20major,dash)), so in practice there may be little to no difference in how the two shebang lines affect script behavior.

## Hardware

#### Why do computers need a CPU and a GPU? What types of tasks are better fitted for one processor or the other?

CPUs excel at processing many varied instruction sets rapidly. GPUs are better suited for a small number of similar but complex instructions involving massively parallel math, like updating graphics displays in real time or executing machine learning.

Strictly speaking, both a CPU and GPU are not needed simultaneously for a computer to function. A CPU could (eventually) process what the GPU does, and vice-versa, but it would be _very_ slow.

see [https://aws.amazon.com/compare/the-difference-between-gpus-cpus](https://aws.amazon.com/compare/the-difference-between-gpus-cpus)
