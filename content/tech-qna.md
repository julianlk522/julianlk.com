+++
title = 'Tech Q&A'
date = 2024-09-04T23:29:32-04:00
draft = true
+++

This page is a collection of questions that I had about various tech and the best answers that I could give now, to my earlier self, to help it make sense.

As always, please [let me know](mailto:jxl1729@miami.edu) if you see something false. I try hard to fact-check my statements but am not perfect.

## General-Purpose Programming

#### What is the point of closures? Why not simply combine the inner and outer functions into one?

I _think_ the answer is that closures allow creating hidden state inside the outer function's lexical scope which is accessible in the inner function but safely sequestered from the rest of your code (which could possibly abuse it if you are not careful). But, importantly, the private state can be dynamically generated with each call to the outer function.

A function containing no other functions _could_ contain its own state, isolated from the rest of the program, but the difference is that it _cannot_ be invoked `n` times to produce `n` different unique states, it is limited to the state declared in its definition. Closures, however, _do_ make it possible to regenerate self-contained state dynamically by calling the outer function, which declares the inner function anew.

#### What is the point of generator functions? In which cases are they preferable to ordinary synchronous functions?

to answer.

#### Are there _exclusive_ use cases for classes compared to functions? Is there a strong reason to use them, aside from just preference or matching some existing code's design themes?

to answer.

#### Why is an in-memory database sometimes preferred to a disk-based database?

No I/O operations for storage or [serialization](https://en.wikipedia.org/wiki/Serialization) for transmission required, so reads / writes are fast.
Not preferred when storage capacity is a concern and RAM is in shorter supply than disk storage, or when persistent changes required.

## SQLite

#### If WAL mode is faster than using a rollback journal, involves less latency from disk I/O operations, and handles concurrent reads/writes better than other modes, why is it not always used and set by default?

[Per the creators of Django and litestream.io/](https://www.reddit.com/r/sqlite/comments/wll1nu/comment/ijx15md/?utm_source=share&utm_medium=web3x&utm_name=web3xcss&utm_term=1&utm_content=share_button), the main reason is to ensure backwards compatibility with older SQLite versions.

[Other disadvantages](https://sqlite.org/wal.html), namely:

1. WAL does not support network filesystems; the database must be accessed only from a single host computer
2. WAL cannot ensure [atomicity](<https://en.wikipedia.org/wiki/Atomicity_(database_systems)>) for transactions involving multiple attached databases
3. WAL is not optimized for systems requiring mostly reads and few writes

#### If creating table indexes speeds up reads, why not always create an index for every column?

1. Consumes more RAM. Exceeding RAM limit of your computer or virtual machine means frequently swapping to / from disk storage, which is slow and more expensive when using a cloud provider.
2. Slows write / delete times. Each index needs to be updated along with its associated columns to stay synced. Waiting longer is no fun.

#### Why _does_ creating a table index improve read performance? Is the primary key not already an index in the traditional sense?

to answer.

## Linux

#### What is the point of having both `apt` and `apt-get` in Debian?
