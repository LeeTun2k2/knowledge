---
tags:
  - database
---
# Page split

=> If table has index, it will always meet page split
=> No index -> no page split

## Page

Page is a smallest unit to store data in database.
When append a new data, but current page is not enough to store -> Split page.

## What happen

Page is full -> Create a new page -> **move 50% data from page A to page B** -> Update pointers & linked list ([[btree+]]).

## Why it consumes resources?

Optimal flow: Write data to tail
Bad flow: Write new data -> way back to read old page -> create new page -> split data -> re-orde index.

=> Cause of fragmentations.

##### Most frequent reason

Use UUID v4 -> random key -> most split page happen

## Related

[[database-indexing]]