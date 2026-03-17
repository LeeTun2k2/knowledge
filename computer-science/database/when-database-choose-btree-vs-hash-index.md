---
tags:
  - database
  - btree
  - hash-index
  - sql
---
# When database choose BTree and Hash Index

## BTree

Is a Tree, Default index data structure of MySQL, Pgsql, Mssql.

Usage:
- Range scans
- Order by
- Prefix matching (LIKE 'abc%')
- Search on a part of composite index. Ex: Index (A, B, C), Query (A, B).
- Time Complexity: O(nlogn)

=> Flexible

## Hash Index

Is a key-value storage built with a hash function.

Usage:
- Equality Search. Ex: (=, IN)
- When required a strong performance.
- Time complexity: O(1)

DO NOT use when:
- Search in Range (not support)
- Order By (not support)
- LIKE query
- Index prefix.

| **Tính năng**          | **B-Tree Index**                 | **Hash Index**                |
| ---------------------- | -------------------------------- | ----------------------------- |
| **Toán tử hỗ trợ**     | `=`, `>`, `<`, `BETWEEN`, `LIKE` | Chỉ `=` và `IN`               |
| **Sắp xếp (ORDER BY)** | Có                               | Không                         |
| **Hiệu suất**          | Tốt ($O(\log n)$)                | Rất nhanh ($O(1)$)            |
| **Lưu trữ**            | Lưu theo thứ tự                  | Lưu không theo thứ tự         |
| **Phù hợp nhất**       | Mọi trường hợp thông thường      | Truy vấn Key-Value tốc độ cao |

## Related

[[zz-btree]]
[[zz-btree+]]