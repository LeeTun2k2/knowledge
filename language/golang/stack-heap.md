---
tags:
  - golang
  - garbage-collector
  - performance
---
# Stack & Heap

## Stack

1 goroutine will create a stack ~ 2kb

Đặc điểm:
- Nhanh
- Bộ nhớ liền mạch
- Tự giải phóng tài nguyên
- Dữ liệu chỉ lưu local, kích thước cố định và vòng đời ngắn (scoped).

## Heap

Vùng nhớ dùng chung cho 1 process (static).

Đặc điểm:
- Chậm hơn stack nhưng linh hoạt hơn.
- do GC quản lý và quyết định khi nào thu hồi tài nguyên.
- Chứa biến lớn và dùng chung cho nhiều goroutine

## Khi nào nằm ở heap, khi nào nằm ở stack

Local variable, pointer -> stack
Global var -> heap
Channel -> heap
Interface -> heap
Oversize variable -> heap

=> Keys Ideas:
- Nếu 1 function return value, pointer -> heap
- Nếu variable không xác định (size, type) -> heap
- Oversize -> heap

## Related
[[garbage-collector]]