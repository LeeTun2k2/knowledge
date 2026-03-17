---
tags:
  - database
---
## Database Connection Pooling

Khi application giao tiếp với database, 2 bên sẽ giữ 1 connection pooling để tối ưu kết nối với nhau.

## Why

Application kết nối với Database thông qua TCP. Khi kết nối cần thông qua các giao thức:
- TCP 3 way handshake
- SSL/TSL
- User Authen - Author.

## HOW

Mỗi khi query, app sẽ mượn 1 connection từ pool để giao tiếp, xong thì trả lại.
Max Pool size: thường bằng CPU * 2

[[database-architecture]]