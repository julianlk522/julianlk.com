+++
title = 'SQL'
date = 2024-11-21T19:29:54-05:00
layout = 'qna-section'
draft = false
+++

## If creating table indexes speeds up reads, why not always create an index for every column?

1. Consumes more RAM. Exceeding RAM limit of your machine means frequently swapping to / from disk storage, which is slow and computationally expensive.
2. Slows write / delete times. Each index needs to be updated along with its associated columns to stay synced.

## How does creating a table index improve read speeds? Is the table's primary key not already an index in the traditional sense?

Primary keys are sufficient as unique identifiers for each row, but they are _not alphabetically sorted_. Indexing sorts the queryable data so that a full table scan is not needed to locate a particular row. A [linear search](https://en.wikipedia.org/wiki/Linear_search) can replaced with a [binary search](https://en.wikipedia.org/wiki/Binary_search), bringing time complexity from **O(n)** down to **O(log n)**. If searching a one-million-row table, the max number of searches to find a target drops from **1,000,000** to **20**.

see [https://www.atlassian.com/data/sql/how-indexing-works#:~:text=Let's,table:](https://www.atlassian.com/data/sql/how-indexing-works#:~:text=Let's,table:)
