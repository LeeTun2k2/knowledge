---
tags:
  - database
  - sql
---
# Database join algorithm

## Nested Loop

use for small table => O(m*n)

## Hash join

use for large table with no index => DB engine auto hash value and then join
=> O(m + n)

## Merge join

all joined columns have indexed => O(m)

## Related
[[database-indexing]]