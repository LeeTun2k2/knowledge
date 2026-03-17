---
tags:
  - indexing
  - database
---
# Covering Index

It is a skill to create index that use all most frequent using columns to create index key.

As knowledge, if the key is cover all necessary columns to SELECT, database will return directly and do not need to access to SELECT object row in table.

## Pros

Quickly access data with specific select columns

## Cons

Take more data to store index keys.

## Related

[[database-indexing]]
[[composite-index]]