---
tags:
  - golang
  - concurrency
---
# Go Schedule

# Go Schedule

## GMT Model
[[gmp-goroutine-machine-processor]]

## Work-stealing

Khi một P hết việc, nó sẽ lấy (trộm) một nửa số G từ đuôi hàng đợi của P khác. Thao tác này dùng tập lệnh nguyên tử (atomic) trên hàng đợi vòng (circular queue) nên cực nhanh.

## Hand-off 

Khi một G thực hiện **System Call (I/O blocking)** làm M bị treo, P sẽ ngay lập tức "bỏ rơi" M đó để đi tìm/tạo một M khác nhằm tiếp tục xử lý các G còn lại trong hàng đợi.

## Preemption

`sysmon` sẽ gửi tín hiệu ngắt nếu một G chạy quá 10ms (như vòng lặp vô hạn). G đó bị đẩy vào **Global Run Queue** để nhường chỗ cho G khác.

## Related

