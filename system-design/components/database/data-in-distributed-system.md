---
tags:
  - database
  - distributed
  - system-design
---
# Data in distributed system

## Problem

Khi data quá lớn, scale chiều dọc quá đắt đỏ + khó + tạo ra downtime.
Query trên 1 dataset nặng bị chậm.
=> Cần phân tán DB trên nhiều node nhưng vẫn đảm bảo tính consistency.

## Replicaion

Tạo database dạng master - slaves.
Luồng ghi đưa vào master.
Sau đó đồng bộ qua slaves.
Slave chỉ phục vụ cho việc read.

Pros:
- Đơn giản
- Khi database bị failed, sẽ có node khác lên thay thế.
- Có thể tối ưu index để phục vụ riêng cho luồng read và luồng write.
- Có thể tối ưu read bằng cách đặt slave ở gần read service, master đặt gần write service.
- Có thể dễ dàng scale theo chiều ngang để tăng conection pool -> giúp tăng throughput cho hệ thống.

Cons:
- Write traffic tập trung tại master.
- Cần thời gian đồng bộ sang slave nên sẽ có động trễ.

Usage:
- Dùng cho hầu hết trường hợp cơ bản.
- Phù hợp cho cụm server đặt tại 1 vị trí duy nhất.


## Multi-master

Gồm nhiều cụm master slave.
Ghi vào cụm này, sau đó đồng bộ sang các cụm còn lại.

Pros:
- Mỗi cụm quản lý 1 domain riêng, hoặc quản lý 1 khu vực địa lý riêng. (dễ sharding)

Cons:
- Conflict dữ liệu khi đồng bộ dữ liệu giữa các master.
=> Đặt độ ưu tiên (super master) để quyết định kiểu dữ liệu.
=> LWW

Usage: 
Thường dùng cho hệ thống càn phân loại dữ liệu theo địa chỉ vật lý.


## Leader less

Mỗi node là 1 replica, hoạt động độc lập nhưng vai trò và lưu trữ dữ liệu như nhau.
=> dữ liệu hoạt động dựa trên cơ chế đồng thuận.

Pros:
- Không sợ down server.
- HA

Nhược điểm:
- Quản lý phức tạp
- Đổi CP lấy AP.

## Related

[[cap-theorem]]