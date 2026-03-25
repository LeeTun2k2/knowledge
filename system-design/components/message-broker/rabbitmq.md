---
tags:
  - rabbitmq
  - system-design
  - message-broker
---
# rabibtmq


## Architecture

Producer -> Exchange -> Binding -> Queue -> Consumer

## Exchange

Direct -> message có exchange key rõ ràng -> đẩy trực tiếp vào queue
Fanout -> fanout -> gửi message broad case đến tất cả queue.
wildcard -> dùng * để định tuyết tất cả queue với prefix và suffix
header -> phân  loại message bằng header.

## ACK / NACK

ACK -> dùng khi consumer xử lý thành công -> pop message và xóa.
NACK -> khi xử lý thất bại -> có thể retry, requeue, skip
Prefetch -> Số lượng message xử lý cùng lúc để fetch về xử lý.

## DLX

Queue riêng biệt để hứng message failed. 

## Quorum Queues



## TTL

Nếu message xử lý quá lâu -> timeout

## Virtual Hosts

Tách env cho dự án, môi trường khác nhau.

