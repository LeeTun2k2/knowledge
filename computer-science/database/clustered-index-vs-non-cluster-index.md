---
tags:
  - database
  - sql
  - indexing
---
# Clustered Index vs non-Clustered Index

## Key Ideas

Clustered Index: Data thực trên bảng phải liền mạch và giữ thứ tự => thường là ID, phải ghi vào đúng vị trí vật lý nên lâu.
Non-Clustered Index: Data ảo, không cần duy trì địa chỉ vật lý, thường ghi vào cuối cho nhanh

## Clustered Index

Change the physical allocation of real data on table.
Key = ID
Value = Leaf
Max instance can create: 1
Read through put: directly
Write: slow (O(logn))

# Non-clustered index

Just change the physical allocation of indexed table. DO NOT change real data address.
Key = (columns, ...)
Value: Clustered-Index
Max instances can create: unlimited
Read: a bit slower (need to look up and read ID first, then read real data)
Write: a bit quicker (no need to maintain real physical allocation of data)

## Related
[[btree]]