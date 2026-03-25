---
tags:
  - system-design
  - database
  - replication
---
# Replication

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

[[data-in-distributed-system]]