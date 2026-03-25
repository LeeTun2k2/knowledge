---
tags:
  - database
  - system-design
  - replication
---
# What happen if master node failed?

Sẽ có 1 service chạy độc lập để health check cụm master slave.
Nếu slave down -> LB chuyển sang slave khác đang avail
Nếu master down -> Bầu chọn 1 master mới từ slave (Election/Promotion)

## Election / Promotion

3 levels theo thứ tự ưu tiên giảm dần:
- Trọng số có sẵn -> chọn node được set trọng số cao nhất.
- Chọn node có data latest
- Chọn ngẫu nhiên.

## Follow up

Data loss: Khi master down, sẽ có 2 loại dữ liệu bị mất
- Dữ liệu đang xử lý -> đặt proxy trước master để ghi log connection. Có thể khôi phục từ connection log.
- Dữ liệu đã lưu nhưng chưa sync sang slave -> đọc WAL để khôi phục

Downtime:
- Trong khi election không thể write, chỉ có thể read từ slave

2 Leaders:
- Leaders không down mà chỉ lag vài phút rồi comeback -> sẽ có 2 leaders
- Vậy cho monitor service shutdown triệt để leader luôn, như vậy sẽ luôn luôn rơi vào trường hợp dataloss.

[[data-in-distributed-system]]
[[replication]]