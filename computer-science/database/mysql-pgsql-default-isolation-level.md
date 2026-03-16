---
tags:
  - acid
  - isolation-level
  - mysql
  - pgsql
---

# MySQL - PgSql isolation level

## Mysql 

Hướng đến replication, Repeatable Read để tránh sync data bị sai.
=> Lựa chọn an toàn cho data

# Pgsql

Dùng thiết kế hiện đại, MVCC -> người đọc không bao giờ chặn người ghi -> hướng tới performance cao.
=> Lựa chọn cho hiệu suất cao.

## Related
[[isolation-level]]