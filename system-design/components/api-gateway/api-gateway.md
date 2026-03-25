---
tags:
  - system-design
  - api-gateway
---
# Api gateway

Là single entry point, proxy backend giao tiếp với internet

## Core Functions

Routing -> /api, /app, /assets, ...
Auth 
Rate limit
Protocol translation -> https to http, https to grpc, web socket to grpc.
Request Aggregation -> gom nhóm request, tạo nhiều subrequest.

## Pros

Giảm độ phức tạp của client -> single entry
Tăng tính bảo mật -> Proxy, không cho client biết backend là ai
Cache -> response trực tiếp nếu request bị duplicate.
Monitor and log -> log tập trung, dễ trace.

## Trade off

Single point failure -> cần triển khai api gateway cluster
latency -> Vì là proxy, tránh xử lý phức tạp và blocking.

