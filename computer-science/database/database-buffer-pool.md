---
tags:
  - database
---
## Buffer pool

Use LRU to cache.

Khi write sẽ ưu tiên ghi vào Buffer pool trước khi ghi xuống disk vì giúp tối ưu cho việc đọc data và dùng ngay.

Write Log trước (để restore đảm bảo tính Durability) -> Write buffer pool (Ưu tiên xử lý data mới) -> Write to disk (Lưu trữ lâu dài.)

## Related

[[database-architecture]]