---
tags:
  - system-design
  - database
  - acid
---
# Isolation Level

| Isolation Level              | Read Lock           | Write Lock           | Ý nghĩa đơn giản                 |
| ---------------------------- | ------------------- | -------------------- | -------------------------------- |
| **Read Uncommitted**         | No                  | Lock write           | **dirty read**                   |
| **Read Committed** (default) | Lock by query       | Lock by commit phase | Chỉ đọc dữ liệu đã commit        |
| **Repeatable Read**          | Lock by transaction | Lock by commit phase | Đọc lại cùng row luôn giống nhau |
| **Serializable**             | Lock by data range  | Lock by commit phase | Sequent transaction              |

## More

Dirty Read: Change value, not commit => read new value
Non-repeatable Read: Change value, not commit => read old data, otherwise read new value
Phantom Read: use aggregated functions to get data -> someone add new data -> use aggregated functions again and read more data than last try.

## Related
[[acid]]