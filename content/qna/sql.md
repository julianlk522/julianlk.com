+++
title = 'SQL'
date = 2024-11-21T19:29:54-05:00
layout = 'qna-section'
draft = false
+++

## If creating table indexes speeds up reads, why not always create an index for every column?

1. Consumes more RAM. Exceeding RAM limit of your computer / virtual machine means frequently swapping to / from disk storage, which is slow and computationally expensive.
2. Slows write / delete times. Each index needs to be updated along with its associated columns to stay synced.

## How does creating a table index improve read speeds? Is the table's primary key not already an index in the traditional sense?

Primary keys are sufficient as unique identifiers for each row, but they are _not alphabetically sorted_. Indexing sorts the queryable data so that a full table scan is not needed to locate a particular row. A [linear search](https://en.wikipedia.org/wiki/Linear_search) can replaced with a [binary search](https://en.wikipedia.org/wiki/Binary_search), bringing time complexity from **O(n)** down to **O(log n)**. If searching a one-million-row table, the max number of searches to find a target drops from **1,000,000** to **20**.

see [https://www.atlassian.com/data/sql/how-indexing-works#:~:text=Let's,table:](https://www.atlassian.com/data/sql/how-indexing-works#:~:text=Let's,table:)

## SQLite

#### If WAL mode is faster than using a rollback journal, involves less latency from disk I/O operations, and handles concurrent reads/writes better than other modes, why is it not always used and set by default?

[Per the creators of Django and litestream.io/](https://www.reddit.com/r/sqlite/comments/wll1nu/comment/ijx15md/?utm_source=share&utm_medium=web3x&utm_name=web3xcss&utm_term=1&utm_content=share_button), the main reason is to ensure backwards compatibility with older SQLite versions.

[A few other catches](https://sqlite.org/wal.html):

1. WAL does not support network filesystems; the database must be accessed only from a single host computer
2. WAL cannot ensure [atomicity](<https://en.wikipedia.org/wiki/Atomicity_(database_systems)>) for transactions involving multiple attached databases
3. WAL is not optimized for systems requiring mostly reads and few writes
