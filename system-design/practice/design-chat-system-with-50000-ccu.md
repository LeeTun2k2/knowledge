---
tags:
  - system-design
  - pratice
---

# Design a chat system with 50000 concurrent users

## Clarify Requirements

Chức năng có gì?
- Chat 1 vs 1
- Chat Group
- Message Status
- Online - Offline
- History

Non functional
- CA

## High level design

Client -> LB -> API GW ->

a. Connection manager (Websocket) -> Online/Offline Status 
b. Chat service (API) -> Chat service -> MQ -> Message Storage
                                 |-> Notification Service

## Deep dive

Q: Tại sao chọn websocket?
Giúp giữ kết nối, không mở lại kết nối nhiều lần gây tốn TCP. Có thể giao tiếp 2 chiều.

Q. Chọn DB gì để lưu message
Chọn Column database là tốt nhất, vì ghi nhiều, search thì theo time range.

Q: Nếu 2 user ở 2 servers khác nhau thì làm sao khởi tạo đoạn chat?
User A bắt đầu chat -> Tạo UNIQUE key -> Lưu vào redis để track session.
Lookup xem B đang ở server nào, yêu cầu server đó Thiết lập web socket connection đến B.

Q: Để đảm bảo tính mở rộng khi thiết lập connection thì sao?
Gửi request đến MQ chung để giảm phụ thuộc.

Q: Với 50k CCU, traffic cực lớn. Làm sao theo dõi status user online?
Mỗi 1 phút, gửi 1 gói heart beat nhỏ để báo với server là user đang online
Với mỗi user status, sét TTL thành 5p đề phòng server die thì status sẽ tự động invalid.

Q: Thiết kế key như nào trong column DB để read và write nhanh?
Dùng cột chat_id sorted(userid1, userid2) làm Partition (đễ dữ liệu được sắp xếp sẵn trên 1 server)
Dùng CreatedAt cho cột Clustering Column để sắp xếp vật lý query cho nhanh

## Bottlenecks & Scaling

Q: 1 group chat has 5k users. How to improve if 1 message was sent?
Tin nhắn quá nhiều => Tạo bảng group để lưu trữ chat. Chat được pagination sẵn và assign vào bucket. Dâta được lưu vào bucket sẽ tránh phải dùng 1 vùng nhớ lớn để lưu dữ liệu cho 1 đoạn chat.

Q: Fan-out
Khi có 1 tin nhắn mới -> notify all user
Khi user mở app lần đầu -> pull bucket về.

Ingest: WebSocket Server nhận tin -> Đẩy vào Kafka/RabbitMQ.
Fan-out Workers: Các worker này đọc tin từ Kafka. Chúng truy vấn DB để lấy danh sách 5.000 thành viên trong group.
Presence Check: Worker kiểm tra Presence Service (Redis) để xem trong 5.000 người đó, ai đang online và ở server nào.
Push: Worker gửi yêu cầu đến các WebSocket Server tương ứng để đẩy tin xuống cho user.

Q: Làm sao đảm bảo thứ tự message
-> Middleware tại BE để assign created date.
-> Id tự tăng -> Snowflake ID

