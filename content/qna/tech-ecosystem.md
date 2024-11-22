+++
title = 'Tech Ecosystem'
date = 2024-11-21T19:29:54-05:00
layout = 'qna-section'
draft = false
+++

## What is RSS? What problem(s) does it solve?

**_RSS_**: _Really Simple Syndication_

That depends on whether you follow a lot of blogs or other (relatively basic) sources that are periodically updated. RSS helps you keep up with those. You configure an _RSS aggregator_ which fetches an XML _sitemap_ file from the sites you specify. The sitemap overviews site content, and your aggregator (or _reader_) will detect and report any updates back to you in a _syndicated_ location (hence the 2nd "S").

With an RSS feed, you can check a single location to get automatic updates from multiple, maybe many, sites at once. But the content must be presentable in XML format, so it won't work well for more sophisticated sites or web-apps, like determining changes to the [Figma](https://www.figma.com/) engine, for example.

## What is LSP? What problem(s) does it solve?

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

## (Lossless) File compression: why is it possible? Doesn't it imply that data is usually stored inefficiently?

**TL;DR**: No, not if both storage space _and_ processing cost are components to efficiency.

Let's clarify lossy vs. lossless compression first because this does not apply to lossy. It's a bit self-explanatory: lossy compression _loses_ some data integrity, for example in MP3 by discarding audio above roughly the [20,000Hz human audible frequency limit](https://interview.orpheus.network/spectral-analysis.php), while lossless compression does not. These offer a key tradeoff: lossy compression increases data reduction (smaller file sizes) but lossless compression allows the exact data to be preserved and reconstructed perfectly after unencoding (higher quality).

Lossy makes sense for storing things like videos. The trimmed data doesn't affect video quality much but has huge implications for storage savings, especially when working on the order of [exabytes](https://what-if.xkcd.com/63/). For other things where exact data representation matter a lot, lossless compression like gzip is required to safely shrink file sizes.

So, if compressing code to a .gz file is possible, why do we ever bother with uncompressed file formats? Isn't a lower number of bits stored always better?

Well, no, because compressed data only makes sense to a decompressing program. For example, with gzip, the underlying [DEFLATE](https://en.wikipedia.org/wiki/Deflate) algorithm replaces each repeated substring in the file with an abbreviated reference to its first appearance, then creates a table where the reference symbols are stored with the shortest possible names (heavily simplified). It completely mangles the original data, from the perspective of a human reader. You need to decompress it to be able to read it again. In the end you've just traded processing time/costs with storage space, which isn't usually worth it.

See [https://www.infinitepartitions.com/art001.html](https://www.infinitepartitions.com/art001.html)
