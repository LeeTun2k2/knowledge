---
tags:
  - database
  - indexing
  - sql
---
# Database indexing

## Clustered index

Real data ordered by this index (Primary Key). 

1 table has only 1 Clustered Index.

## Non-clustered index

Key: index valuable to look up
Value: pointer to cluster index.

## Covering Index

If a select command get columns in index, return directly and skip look up table.


## More

Selectivity => Nếu giá trị của cột không đa dạng, index bị kém hiệu quả nên engineer sẽ chọn table scan.

## Related
