---
tags:
  - http
  - network
  - tcp
---

| **Đặc điểm**        | **HTTP/1.0**         | **HTTP/1.1**             | **HTTP/2**                   | **HTTP/3**                       |
| ------------------- | -------------------- | ------------------------ | ---------------------------- | -------------------------------- |
| **Protocol**        | TCP                  | TCP                      | TCP                          | **UDP (QUIC)**                   |
| **Kết nối**         | Đóng sau mỗi request | Giữ kết nối (Keep-alive) | Duy nhất 1 kết nối           | Duy nhất 1 kết nối               |
| **Cơ chế truyền**   | Văn bản thuần (Text) | Văn bản thuần (Text)     | **Nhị phân (Binary)**        | **Nhị phân (Binary)**            |
| **Xử lý song song** | Không (Chờ đợi)      | Pipelining (Dễ lỗi)      | **Multiplexing**             | **Multiplexing**                 |
| **Nén Header**      | Không                | Không                    | Có (HPACK)                   | Có (QPACK)                       |
| **Ưu điểm chính**   | Đơn giản             | Phổ biến, dùng Pool      | **Cực nhanh, hiệu suất cao** | Không bị nghẽn (HOL) khi mất gói |

## Releated

[[http]]
[[multiplexing]]