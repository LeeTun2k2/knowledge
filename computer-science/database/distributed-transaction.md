---
tags:
  - database
  - distributed
---
# Distributed Transaction

## Problem

Có 2 database, mỗi database có 1 bảng khác nhau và có relation với nhau.
Khi update, cần tạo transaction giữa 2 bảng. Làm thế nào khi 2 database khác nhau. 

## Solution

##### 2 Phase commit

Mở transaction ở cả 2 DB.
Thực hiện các lệnh
Nếu tất cả thành công -> commit
Nếu tất cả thất bại -> Roll back


Pros:
- Dễ
Cons:
- Tốn resource vì phải giữ 2 connection.

##### Saga pattern

Hoạt động giống migration, chia transaction thành các transaction nhỏ, mỗi transaction nhỏ có 2 action là execute và revert.

## Related

[[zz-2-phase-commit-vs-saga-pattern]]
[[transaction-locking]]