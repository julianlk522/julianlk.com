+++
title = 'Web Development'
date = 2024-11-21T19:29:54-05:00
layout = 'qna-section'
draft = false
+++

## When designing client-server web APIs, when might you want to choose REST, REST + GraphQL, or RPC? What are the unique benefits of each style?

#### [TL;DR](#with-that-said)

First, an overview of each:

#### REST

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

#### GraphQL

-   Not just a style and not autonomous: GraphQL is a request/response simplification language. It runs on the same server as an existing API to translate [declarative](https://en.wikipedia.org/wiki/Declarative_programming) query/mutation/_subscription_<sup>[\[3\]](#subscription-ref)</sup> instructions into requests to that API, consolidating its responses into unified graphs which are returned to the client.
    -   Effectively merges multiple API calls into one
    -   Removes need to keep track of numerous endpoints since all requests originate from the same one
-   Adheres to a pre-defined API schema
    -   Enforces request operation and input validity through strong data typing
        -   Helps to improve error message specificity
    -   Prevents overfetching; ensures responses contain only specifically requested data, omitting excess from the underlying API's responses
    -   Allows procedurally generating API documentation through schema [introspection](https://spec.graphql.org/draft/#sec-Introspection).

#### RPC

-   Allows direct access to calling server functions as if executed locally
    -   By default offers high level of freedom in executed actions
-   HTTP connections are not as ubiquitous as with REST, though are used in many implementations e.g., gRPC
    -   Alternative implementations may involve only requests and asynchronous handling, e.g., JSON-RPC, instead of HTTP-like synchronous request/response cycle
-   Can be stateful
-   Implementations vary and are prone to incompatibility across different client types
    -   Though may circumvent this with a standardized [Interface Description/Definition Language (IDL)](https://en.wikipedia.org/wiki/Interface_description_language) to create cross-system (language, OS) compatibility

#### With that said...

#### Which APIs might "REST" (or an approximation of it) be best suited for?

In as simple a description as possible: ones prioritizing reliability and broad, long-term compatibility.

-   Confining possible interactions through specific URI definitions
-   Clarifying availability of URIs via hypermedia links in returned responses
-   Compatibility across varying client types, especially for World Wide Web access

#### Which APIs might benefit most from GraphQL?

In as simple a description as possible: the most complex, resource-intensive ones.

-   Sprawling APIs with complex consumption patterns where the requested data must come from combinations of endpoints
-   APIs using many endpoints that are hard to keep track of
-   APIs with heavy request loads where overfetching data is a significant expense

#### Which APIs might prefer to use RPC?

In as simple a description as possible: ones responsible for private, high-trust actions with a greater priority on performance over broad support. e.g., [microservice](https://en.wikipedia.org/wiki/Microservices)-based backends.

-   Freedom to execute server actions directly can be a security vulnerability. It is less of a concern if the RPC endpoint is kept private.
-   Non-standardization has the benefit of optionally neglecting delivery of non-essential data, e.g., HTTP response status codes, which can improve processing efficiency at the expense of interoperability. This is a worthwhile trade-off if interoperability doesn't matter much.

<a name="www-ref"></a>[1]. The World Wide Web has a distinct meaning from the Internet. The Internet describes the global network of computer networks that use packet-based routing to exhange data; the World Wide Web describes a system of Uniform Resource Identifiers (URIs) that index various data. See [https://www.w3.org/help/#webinternet](https://www.w3.org/help/#webinternet).

<a name="URI-ref"></a>[2]. As a side note, a URI is a unique resource identifier, and a URL is a URI that includes the resource's required access protocol. Since we know the protocol here is HTTP(S), it is most accurate to say "URI." See [https://danielmiessler.com/p/difference-between-uri-url/](https://danielmiessler.com/p/difference-between-uri-url/).

<a name="subscription-ref"></a>[3]. A subscription is a long-lived request that continues to fetch data automatically in response to each occurrence of a specified event.

see:

-   [https://cloud.google.com/blog/products/application-development/rest-vs-rpc-what-problems-are-you-trying-to-solve-with-your-apis](https://cloud.google.com/blog/products/application-development/rest-vs-rpc-what-problems-are-you-trying-to-solve-with-your-apis)
-   [https://www.baeldung.com/cs/rest-vs-http](https://www.baeldung.com/cs/rest-vs-http)
-   [https://stackoverflow.com/a/26049761](https://stackoverflow.com/a/26049761)
-   [https://www.visual-paradigm.com/guide/development/what-is-rest-api/](https://www.visual-paradigm.com/guide/development/what-is-rest-api/)
-   [https://spec.graphql.org/draft/#sec-Overview](https://spec.graphql.org/draft/#sec-Overview)
-   [https://www.howtographql.com/basics/1-graphql-is-the-better-rest/](https://www.howtographql.com/basics/1-graphql-is-the-better-rest/)
