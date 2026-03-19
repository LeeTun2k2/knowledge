---
tags:
  - network
  - epool
---
# Epool


## Problem

Nếu không có epool, OS muốn kiểm tra trạng thái của TCP phải pooling để check, mỗi lần check cần loop qua tất cả TCP để lấy status

## Solution

ePool -> event listener khi TCP thay đổi status. -> Giúp đỡ tốn tài nguyên.