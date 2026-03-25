---
tags:
  - concurrency
  - work-experience
---
# How to handle burst of concurrent connection to a single url

## Frontend side

Nếu là static -> dùng front end là SSG để tận dụng browser cache, phân phối qua CDN.
Nếu data động nhưng ít thay đổi -> dùng client side caching.

## Infrastructure side

Dùng caching như Redis.
Chuyển luồng read sang các replica trong master-slave
Ratelimit để chống user spam on high trafic.
Message queue để delay trafic.

## Application side

Local hot key small caching => local cache nhỏ nhẹ tại backend, dùng LFU để cache lại data dùng thường xuyên nhất. LFU sẽ là small snapshot của redis.

Chủ động chạy worker để cập nhật cache nếu data thay đổi thường xuyên.

Deduplicate (Request Collapsing) -> lock lại các request giống nhau, chỉ cho 1 request đi qua để lấy kết quả thật sự, sau đó lấy kết quả đó trả cho toàn bộ request bị trùng. Có thể kết hợp thêm 1 chút debounce để tránh bị lock nhiều lần.

Nếu chấp nhận độ trễ => dùng semaphore để giới hạn số request thực sự chạy.

Giảm GC (vì tạo nhiều request nên object cũng tạo nhiều):
-> Tránh dùng heap, ưu tiên dùng stack. (struct, value type thay vì class, reference type)
-> Tạo object pool -> 
-> Tạo memory pool -> 


