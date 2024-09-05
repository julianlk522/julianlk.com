+++
title = 'Tech Q&A'
date = 2024-09-04T23:29:32-04:00
draft = true
+++

This page is a collection of questions that I had about various tech and the best answers that I could give my earlier self to help understand.

## General-Purpose Programming

#### What is the point of generator functions? In which cases are they preferable to ordinary synchronous functions?

to answer.

## What is the point of closures? Why not simply combine the inner and outer functions into one?

I _believe_ the answer is that closures allow the outer function to create hidden state which is only accessible in the inner function and is safely sequestered from the rest of your code (which could possibly abuse it if you are not careful). They help avoid bugs.

#### Are there _exclusive_ use cases for classes compared to functions? Is there a strong reason to use them, aside from just preference or matching some existing code's design themes?

to answer.

## SQLite

#### If WAL mode is faster than using a rollback journal, involves less latency from disk I/O operations, and handles concurrent reads/writes better than other modes, why is it not always used and set by default?

[Per the creators of Django and litestream.io/](https://www.reddit.com/r/sqlite/comments/wll1nu/comment/ijx15md/?utm_source=share&utm_medium=web3x&utm_name=web3xcss&utm_term=1&utm_content=share_button), the main reason is to ensure backwards compatibility with older versions.

[Other disadvantages](https://sqlite.org/wal.html), namely:

1. WAL does not support network filesystems; the database must be accessed only from a single host computer
2. WAL cannot ensure [atomicity](<https://en.wikipedia.org/wiki/Atomicity_(database_systems)>) for transactions involving multiple attached databases
3. WAL mode is not optimized for systems requiring mostly reads and few writes
