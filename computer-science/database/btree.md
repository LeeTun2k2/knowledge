---
tags:
  - btree
  - database
  - sql
---
# Btree

Self-balance tree
Multi-branches

## Pros

Self-balance -> Keep the median READ, WRITE time balance.
Multi-branches -> Small depth -> Reduce disk IO -> high through put
Node size usually = 1 page -> 4kb or 8kb -> quickly search.
Quick Search O(logn) instead of O(n) of Full table search

## Cons

Slow write O(logn) instead of O(1) when append to tail of file.

## Related

[[database-indexing]]