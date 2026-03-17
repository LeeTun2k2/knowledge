---
tags:
  - database
  - sql
  - nosql
---
# Compare SQL vs No-SQL

## SQL

Structure data shape -> Dữ liệu có cấu trúc
ACID -> Hướng đến Consitency
Phù hợp: dữ liệu ổn định, có cấu trúc -> Giảm exception ở tầng Application
Dữ liệu thường normalize để giảm kích thước và thể hiện mỗi quan hệ với nhau. 
Join -> Vì là dữ liệu có quan hệ -> Complex join và tổng hợp.
## No SQL

Scaling and Flexible -> Dữ liệu linh hoạt, dễ mở rộng
BASE -> Hướng đến Availability
Phù hợp: Dữ liệu thường xuyên thay đổi format. -> Không phải thay đổi cấu trúc nhiều.
Dữ liệu thường denormalize để read được những gì thật sự cần. -> Thiết kế để tối ưu read
Được thiết kế cho sharding. Dạng key-value hỗ trợ tốt, read cho độ trễ thấp.

## Related

[[acid]]
[[base]]