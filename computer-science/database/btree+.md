---
tags:
  - btree
  - indexing
  - database
---
# BTree+

## Key Ideas

Advance version of BTree
Common usage for Pgsql, Mysql, Mssql.

Only leaf save data, other node is only pointer. => Reduce disk access
Pointer rất nhẹ. 1 node (page) có thể chứa hàng nghìn pointer. -> giúp giảm chiều cao của cây xuống => giảm truy cập disk.

Leaf is a doubly linked list => Optimize for:
- Better for table scan
- Better range query

## Related

[[btree]]

