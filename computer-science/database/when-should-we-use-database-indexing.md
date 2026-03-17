---
tags:
  - database
  - indexing
---
# When should we use database indexing?

## When prefer to use

Frequently use WHERE, JOIN with this columns.
Frequently ORDER BY, GROUP BY. Good to use with BTree.
When data is a large set, full table scan is too slow.
When columns have high selectivity.

## When not prefer to use

Table is too small, use index is over-engineer.
Write too frequently.
Low selectivity.
Cell data is too large. <- Index will create a copy of cell data to build index, so if cell data is too large, it will add more pressure to CPU and space complexity.

## Related

[[btree]]
[[database-indexing]]