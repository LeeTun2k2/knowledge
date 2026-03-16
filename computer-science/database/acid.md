---
tags:
  - database
  - sql
---

# ACID
## Atomicity

A transaction performs a sequence of operations that must be treated as a single unit. If any part fails, the entire transaction is aborted.
## Consistency

Transitions the database from one valid state to another.
This ensures that all **Integrity Constraints** (Data types, Foreign Keys, Unique constraints) are met.

## Isolation

Prevents concurrent transactions from interfering with each other.
Has 4 Isolation levels: 

| **Cấp độ**           | **Hiện tượng có thể gặp** | **Ngăn chặn**                   | **Cách hoạt động**                                                                | **Nhược điểm**                                                                         |
| -------------------- | ------------------------- | ------------------------------- | --------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------- |
| **Read Uncommitted** | Dirty Read                | Không ngăn chặn gì              | Đọc trực tiếp dữ liệu từ bộ nhớ đệm, kể cả khi chưa lưu chính thức.               | Dữ liệu không đáng tin cậy vì có thể bị hủy (rollback) bất cứ lúc nào.                 |
| **Read Committed**   | Non-repeatable Read       | Dirty Read                      | Chỉ cho phép đọc các dữ liệu đã được xác nhận (Commit).                           | Trong cùng một phiên làm việc, đọc một dòng hai lần có thể ra kết quả khác nhau.       |
| **Repeatable Read**  | Phantom Read              | Dirty Read, Non-repeatable Read | Khóa các dòng dữ liệu đã đọc hoặc dùng bản sao (snapshot) cố định cho suốt phiên. | Không ngăn được việc xuất hiện các dòng mới (bóng ma) khi thực hiện lệnh thống kê/đếm. |
| **Serializable**     | Không có                  | Tất cả các hiện tượng           | Các giao dịch được xếp hàng và thực thi lần lượt như thể chạy đơn luồng.          | Hiệu năng hệ thống giảm mạnh, dễ gây tắc nghẽn và chờ đợi lâu.                         |

## Durability

Permanence after a commit.
**Mechanism:** Guarantees that even in a system crash or power failure, the results of a committed transaction are recorded in non-volatile memory (disk).

