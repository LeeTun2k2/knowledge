---
tags:
  - database
  - sql
  - performance
---
# Database level scan

## Level

**Full table Scan**: scan end to end -> waste resources, slow
**Index Scan**: scan index end to end -> use less resources than full table scan -> medium
**Index seek**: Access data directly through BTree -> best performance.

## How database chooses access level?

**Full Table Scan:** 

- SELECT * FROM TABLE (without WHERE)
- SELECT * FROM TABLE WHERE A = B (column A was not indexed)
- SELECT * FROM TABLE WHERE A = B (column A was indexed but small selectivity (get a list ~30% rows of table))
- Table is too small -> scan all should be better.

**Index Scan:**

- SELECT * FROM Table WHERE A = B (A was indexed in (C, A) but wrong ordered)
- SELECT COUNT(A) FROM Table (A was indexed) -> Aggregate function in a indexed column

**Index Seek:**
- WHERE, JOIN use correctable index column and order.
- Access data directly.