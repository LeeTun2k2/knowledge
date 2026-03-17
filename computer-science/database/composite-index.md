---
tags:
  - database
  - indexing
---
# Composite Index

## Leftmost prefix rule

This is the core concept of composite index.
If indexed on (A, B, C)
- WHERE (A, B, C) -> hit
- WHERE (A) -> hit a part
- WHERE (A) -> miss

## Order prefix rule

- High selectivity -> low can let database skip index lookup.
- Most frequenty use -> follow the advantage of leftmost prefix rule
- Range select -> use at right most

## Design index rule

DO NOT create a lot of single index -> improve by using Leftmost prefix rule.


## Related
[[database-indexing]]