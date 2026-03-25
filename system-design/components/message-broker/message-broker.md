---
tags:
  - system-design
  - message-broker
---
# Message broker

Tách tác vụ nặng trên main thread và đưa xuống chạy background.

## Components

Publisher
Queue
Consumer.

## Why message queue

Decoupling -> Đưa message vào queue, tự consumer lấy message ra xử lý -> không cần biết consumer là ai, giảm phụ thuộc.
Scalability -> Tăng consumer lên để xử lý cho nhanh.
Resilience -> Consumer bị crash thì message vẫn an toàn trong queue.
Smoothing overloads -> Khi bị burst traffic thì queue giúp giảm tải.

## Model

P2P: 1 publisher, 1 queue, 1 consumer -> đơn giản, dùng để tách tác vụ nặng hoặc delay traffic.
Publish/Subcribe: 1 publisher - nhiều consumers -> fanout, dùng cho thông báo, ...
