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

Q: Làm sao xử lý trạng thái tin nhắn (Sent, Delivery, Receive, Read)?
Trong 1 group chat, chỉ track status của tin nhắn latest.

Q: Làm sao để track message trong group đã có những ai đọc rồi?
Chấp nhận độ trễ, client gom message theo batch để gửi request update status. Message được đưa vào batch và update từ từ vào database. 
User muốn xem ai đã đọc thì call api, với interval thấp.

Q: Nếu user cần gửi hình ảnh hoặc video thì sao?
1. **Client-Side:** Thay vì gửi byte qua WebSocket, Client gọi một **REST API** (POST `/media/upload`).
2. **Media Service:** Service này nhận file, thực hiện các tác vụ như:
    - **Validation:** Kiểm tra loại file, kích thước.
    - **Processing:** Tạo ảnh thu nhỏ (Thumbnail), nén ảnh để tiết kiệm băng thông.
    - **Storage:** Đẩy file gốc và thumbnail lên **Object Storage** (S3/GCS/MinIO).
3. **URL Metadata:** Media Service trả về một `media_id` hoặc một `signed_url`.
4. **Message Delivery:** Client sau đó mới gửi một tin nhắn văn bản qua **WebSocket** có nội dung chứa cái URL đó.

Q: Nếu upload file nặng, client và media service phải treo connection để handle thì tốn resource. Có cách nào tối ưu?
Trả pre-sign url => Khi media service nhận được request, lập tức tạo 1 valid url sau đó trả lại cho client và chuyển upload connection cho worker để chuyển tiếp đến object storage.

## API Design

##### Auth and refresh token

POST /api/v/auth/login
POST /api/v/auth/refesh

##### New chat
POST /api/v/chat/new {list of userid}

##### History
GET /api/v/group/list : list of top private chat & group chat
GET /api/v/group/{id}/messages: top k message from group


## Database design

##### SQL 

Các thông tin quan trọng như User, Group phải lưu trong SQL để đảm bảo consistency.

##### Column DB

Các thông tin như chat lưu trong Cassandra để query range và write cho nhanh.

##### Redis

Để cache lại session, connection, bucket, ...


## Scaling

Q: Nếu từ 50k -> 1M CCU thì sao?

Websocket chịu không nổi -> Cần LB để chia tải
Redis chịu không nổi -> Redis cluster tại server, local cache tại client.

Q: Hiện tại đang dùng api v1, nếu nâng lên api v2 thì sẽ lost toàn bộ connection. Vậy phải làm sao?
Zero downtime blue green solution => Migrate từ từ từng node theo quy trình: Deploy version mới => Check ổn định => Down version cũ.
Yêu cầu server cũ delay khi tắt khoảng 30 phút để xử lý các message cũ, đến khi hết message thì mới tắt.
Sử dụng LB để điều hướng connection từ v1 sang v2.
Phía client phải có exponent backoff, nếu bị down do server upgrade thì phải retry với exponent time.
Thiết kế client side message pool, lưu các tin nhắn gần đây và status đã gửi hay chưa. Nếu status chưa gửi (server chưa ACK) thì cần implement retry 