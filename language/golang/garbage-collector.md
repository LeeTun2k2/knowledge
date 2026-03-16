---
tags:
  - golang
  - garbage-collector
---
# GC

## Algorithm

##### Tri-color Mark & Sweep

White -> Chắc chắn xóa
Grey -> weak-reference -> các đối tượng đã được quét nhưng reference tới nó thì chưa
Black -> Chắc chắn giữ

## GC life-scope

1. Mask setup -> tạm dừng (write barrier) chương trình (rất ngắn).
2. Masking -> scan heap tree
3. Mark Termination -> đánh dấu tri-color
4. Sweeping -> release white

## Trigger

Thủ công: runtime.GC()
Auto: 
- Giới hạn cứng 
- Dynamic -> dựa vào memory của lần GC cũ, nếu gấp đôi thì trigger.
Khi gặp Fragmentation, Segment fault (không đủ bộ nhớ để cấp phát dữ liệu mới)
