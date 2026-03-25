---
tags:
  - message-broker
  - rabbitmq
  - kafka
---
# Compare rabbitmq vs kafka

|             | RabbitMQ                                                                             | Kafka                                                                                    |
| ----------- | ------------------------------------------------------------------------------------ | ---------------------------------------------------------------------------------------- |
| Model       | MQ manage message state                                                              | Consumer manage message state                                                            |
| Storage     | ACK = delete                                                                         | Save on disk                                                                             |
| Transfering | Push base                                                                            | Pull base                                                                                |
| Msg Order   | Khó đảm bảo. Cần tạo nhiều queue hoặc hashing để tự phân phối                        | Partition, partition key                                                                 |
| Performance | Trung bình, Nghìn requests/ giây                                                     | High (triệu request/s)                                                                   |
| Độ phức tạp | Thấp                                                                                 | Cao                                                                                      |
| Usage       | Routing phức tạp<br>Traffic trung bình<br>Độ tin cậy cao<br>Background task đơn giản | Xem lại message cũ (message replace)<br>High traffic<br>Streaming<br>Through put cao<br> |
[[rabbitmq]]
[[kafka]]
[[message-broker]]