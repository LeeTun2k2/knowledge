---
tags:
  - database
---
## Database Architecture

## Physical Storage Manager

Smallest unit is Page -> 8KB

## Buffer Pool

Caching layer between DISK and RAM. use LRU cache.

##### Pinning 
-> Pin current page to top -> quickly access

##### Dirty Tracking
-> Manage and invalid cache if changed.

## Recovery Manager - WAL (Write Ahead Logging)

Đảm bảo Durability trong ACID.
Write log trước khi ghi vào page.
Append only -> O(1)

## Index Manager

B+ Tree -> Balance tree, multi-branches, store value at leaf, other node store pointer, leaf has double linked list to optimize table scan.

## FLOW WRITE

1. Check Index Manager, tìm page phù hợp
2. Buffer Pool load page lên
3. Write Log WAL
4. Write Disk (Storage Manager)


## Database Concurrency

Shared-Lock: Cho phép nhiều người cùng đọc.
Exclusive-Lock: Chỉ cho phép 1 người được write.
Multi-version concurrent control.

## Related

[[isolation-level]]
[[btree+]]
[[database-buffer-pool]]