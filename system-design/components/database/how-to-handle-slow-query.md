---
tags:
  - database
  - work-experience
---
# How to handle slow query.

## Detect

Ở tầng application, đặt logs để trace xem service nào chậm, có thể dùng profiling.
Ở tầng database, bật slow query log để xem query nào bị chậm

## Analyze

Dùng công cụ explain -> DB sẽ phân tích ra các bước thực hiện query. Show rõ bước nào chậm, thời gian bao nhiêu, tỉ lệ miss/hit cache, memory sử dụng.

## Optimize

Indexing
-> Check xem đánh index có hợp lý không? dùng composite, covering index. Phân tích bảng này có write nhiều không để quyết định.

Partition nếu query có sửa dụng range.

Tối ưu query
- Tránh SELECT *
- Tránh hàm tổng hợp khi query
- Tránh N + 1 problem

## Nếu không thể optimize thì sao?

Restructure database nếu thiết kế tệ.
Nén những data cũ không dùng nữa ra khỏi bảng.
Thêm RAM cho buffer pool để tăng caching, giảm IO

[[covering-index]]