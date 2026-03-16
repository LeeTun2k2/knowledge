---
tags:
  - golang
  - goroutine
  - concurrency
---
# Go Channel

Channel is a thread-safe queue + synchronization primitive
Main objects: sender - buffer / unbuffer channel - receiver
## Unbuffered channel

Có capacity = 0. có thể gửi vô hạn goroutine vào.
Không save data, only transfer and sync goroutine from sender and receiver

**Nếu không có reveiver** => deadlock 

## Buffered channel

Là 1 concurrent queue. Có thể đẩy goroutine vào miễn là channel chưa đầy. Khi channel đầy -> block cho đến khi trống.

## Select

cho phép chờ nhiều channel cùng lúc.

## Pros

safe concurrent queue
built-in sync
code dễ đọc hơn

## Cons

deadlock nếu cố lấy ra khi channel không có gì.
buffer channel khi đầy sẽ block bất ngờ.

## Question

##### Các hiện tượng của channel?

Không có receiver -> block
Nếu không có data nhưng vẫn listen -> block
Nếu buffered channel đầy nhưng vẫn send data vào -> block

Khi tất cả goroutine đều bị block -> deadlock
##### Khi nào unbuffered nhanh hơn, khi nào buffered nhanh hơn?

Khi sender burst nhiều goroutine cùng lúc -> buffered channel nhanh hơn vì ghi tạm goroutine vào buffer và đi làm task khác.

Khi sender tạo goroutine tuần tự -> unbuffered channel nhanh hơn vì sender transfer goroutine trực tiếp cho receiver, skip được chi phí cho việc ghi goroutine vào buffer.

## Related

[[goroutine]]