---
tags:
  - caching
  - system-design
  - redis
---
# Redis

Là key-value storage, tập trung vào read - write performance

## Usage

Caching 
Session storage
Distributed lock
Real-time analystic
Message broker
Ratelimit

## Key Concepts

Memory Storage -> Read / Write nhanh
Single thread -> Không gặp vấn đề của concurrency
Persistence -> Lưu snapshot xuống đĩa lâu dài, dễ dàng khôi phục
Cluster & Sentinel -> Distributed caching

