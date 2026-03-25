---
tags:
  - sharding
  - database
  - system-design
---
# Sharding

Giúp giải quyết bài toán 2 bài toán:
- Có quá nhiều dữ liệu 1 server không lưu hết
- Bottleneck của system là network do khoảng cách địa lý.

## Cách sharding

Theo key cụ thể: ví dụ quốc gia, vùng, tenant, alphabet, ...
Theo id (ngẫu nhiên) và chia đều
Theo cụm nhiều cột (cần thuật toán hash)


## Thêm, xóa node như thế nào

Xem hash là 1 circle linked list
=> Thêm 1 node vào giữa 2 nodes -> rehash 2 nodes bị chen vào giữa, không cần rehash lại toàn bộ.