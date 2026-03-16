---
tags:
  - golang
  - error
  - exception
---
# Error handling

Là cơ chế xử lý lỗi, khá giống try catch

## Compare with try catch

| Try-catch                             | Error                                |
| ------------------------------------- | ------------------------------------ |
| Nếu gặp lỗi, break block try để xử lý | Buộc xử lý lỗi chủ động ngay lại chỗ |
| Nặng vì cần tạo stack trace.          | Nhẹ                                  |
| Dùng catch để bắt lỗi                 | Dùng if để bắt lỗi                   |
| Xử lý unexpected = Exception          | Panic                                |
| có thể trả exception ở global scope   | Phải trả lỗi qua từng tầng           |
## Panic

Nếu panic xảy ra, function dừng ngay lập tức, stack sẽ close cho đến khi tìm được defer.

Nếu recover -> dừng panic và quay lại trạng thái trước đó, chạy như thường.
Nếu không recover -> trả panic lên hàm cha, cho đến khi gặp defer. Nếu không có defer thì crash app.

## Khi nào sử dụng

Dùng panic để dừng ứng dụng sớm khi thiếu config quan trọng.

Dùng recover trong middleware để tránh crash app.