---
tags:
  - database
  - sql
---
# Database Execution plan

## Table scan

scan all table => O(n)

## Index scan 

Find key in index look up => get pointer and then load row data
=> O(nlogn)

## Index Only Scan

Find value in index lookup and return value directly. SKIP load row data.
=> best performance

## Related
[[database-indexing]]
