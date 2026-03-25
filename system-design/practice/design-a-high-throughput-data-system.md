---
tags:
  - database
  - system-design
  - pratice
---
## Clarification

Định dạng dữ liệu: raw, compress, media
Khối lượng: Khổng lồ, nhiều Peta byte
Tần suất write: 
- Import full: chạy 1 lần
- Import CDC: đồng bộ nhanh, thời gian không quá 30s.
Tần suất read: Nhiều
Thời gian lưu trữ: có thời hạn, vĩnh viễn.
Ai là người import? admin.
Import cái gì?

## Nhận xét

Hệ thống data lớn, lưu trữ lâu dài. Import với throughtput cao, Yêu cầu độ trễ thấp, chấp nhận eventual consistency.

## High level design

Client -> API Gateway -> Data verification service -> MQ

MQ -> Import data service -> Database Master (sharding) -> service -> Database replication (sharding) -> Report service dùng slaves.

## API design

##### Upload file lớn
- POST /api/v1/upload {file, request key, sequent number, isEnd} -> Khi upload 1 file, verify + dùng redis để đếm. Khi gặp is end thì mới gửi message đến MQ.

Collect log nhỏ:
- POST /api/v1/log/push {data} -> verify -> MQ

##### Import data service

Read file streaming để collect dữ liệu theo batch, sau đó import

Database:
Dùng column database để làm storge chính.
Dùng Redis để lưu metadata của các file được import vào.
Với lượng dữ liệu siêu lớn như vậy, cần xây dựng hệ thống sharding.
Đặt 1 LB trước cụm database để chia tải cho node.
Chọn hash function để gom data theo bucket liên tiếp, mỗi bucket size 1000.
Sau khi verify dữ liệu từ api (dùng head pointer streaming) thì dùng api gateway chuyển connection sang object storage để ghi.

## Deep dive

Q: Chọn key để sharding như thế nào? -> Hỏi lại xem là report sẽ lưu dữ liệu như thế nào?
- Chọn theo thời gian -> Một số ngày nhiều, một số ngày ít
- Chọn theo id -> Dữ liệu đều nhưng query phức tạp vì phải scan tất cả node.
- Composite key: (id, thời gian) -> vừa group được một nhóm theo query, vừa group được theo thời gian.

Q: Làm sao để truy vấn (Read) không phải quét toàn bộ các node?
Kết hợp sharding và partition.
sharding theo (id + timestamp) -> chia đều data.
Partition theo (timestamp) -> sort sẵn data để lấy cho nhanh.

Q: Nếu query tất cả node như vậy thì lấy dữ liệu như nào cho nhanh?
Dùng Distributed Query Engine
-> Nhận request, query và xử lý trực tiếp trong internal, trả kết quả ra ngoài.