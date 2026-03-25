---
tags:
  - message-broker
  - system-design
---
# How to keep message order in message queue?

## Kafka like - Partition

Chia nhỏ queue thành các partition. 
Partition luôn luôn giữ thứ tự queue FIFO,
Message cần giữ thứ tự thì gắn partition key. Message vào trước luôn được xử lý trước.
Chỉ có 1 consumer gắn vào 1 partition. -> tránh xử lý sai lệch thứ tự.
-> Hiệu suất cao

## Rabbitmq like - FIFO queue

Là 1 queue nhưng giữ thứ tự nghiêm ngặt, chỉ cho phép 1 consumer truy cập vào
-> vấn đề: Xử lý tuần tự, chậm.

## Xử lý tại tầng Application

Mỗi pipe line cần giữ thứ tự thì gắn order key vào header của request.
Nếu message 2 tới sớm hơn message 1 thì backend chủ động hold/skip message 2 và chờ message 1 đến để xử lý.
-> Tốn buffer.

## Design layer

Thay vì gửi PATCH để update (thực hiện operation cộng, trừ, ...), gửi PUT (expectation) để ghi đè. 
-> Thứ tự của message không còn quan trọng.
-> Vấn đề: cần check xem giá trị expectation có đúng hay không.

## Problem

Idempotency: 1 message nhưng gửi 2 lần
-> dùng Idempotency key để track session và order.

**Retry Mechanism:** Tin 1 lỗi và được gửi lại (retry), trong khi tin 2 đã xử lý thành công.
-> Nếu message 2 đến mà chưa message 1 chưa xử lý xong thì cho message 2 failed và retry with exponent time.

Multiple Consumers: Dùng post-keybase hashing với thuật toán hashing cố định. Sẽ đảm bảo cùng 1  key thì message chỉ đi vào 1 consumer.

[[message-broker]]