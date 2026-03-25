---
tags:
  - redis
  - caching
  - system-design
---
# How  redis sync data

Có SOURCE và TARGET
Redis GET từng key từ SOURCE và đưa xuống thread phụ
thread phụ nén lại và gửi sang TARGET
TARGET thread phụ giải nén và đưa lên Thread chính
Thread chính ghi dữ liệu vào

## Issues

CPU overhead nếu key lớn.
migrate lượng lớn có thể gây ra blocking.