---
tags:
  - concurrency
  - parallel
---
# Compare concurrency vs parallelism

## Concurrency

Break task to small unit. Chuyển đổi qua lại để thực hiện task giúp tránh blocking => tối ưu thời gian chạy, giảm thời gian idle của CPU

## Parallelism

Thực hiện nhiều việc cùng lúc.

## Question

##### 1 CPU thì sao?
-> concurrency bình thường
-> Không parallel được.

##### Nếu chạy song song unlimited task thì có nhanh hơn không?

Không. Tốc độ phụ thuộc vào số lượng worker.

## Related
