---
tags:
  - system-design
  - load-balancer
---
# Load Balancer

Là 1 components trong system design giúp điều phối và cân bằng tải.
=> Tăng tính HA và scalability.


## LB Algorithms

Round Robin: Chia đều (đơn giản nhất)
Weighted round Robin: Chia theo trọng số.
Least connection: Chọn server đang ít kết nối nhất (ưu tiên rảnh)
IP Hash: Điều phối ip vào 1 server cố định (hoặc 1 key khác bất kỳ)
Adaptive connection: Dựa vào tình trạng CPU, RAM, throughput để chia tải. 

## LB Layers

Layer 4 (Transport): Điều phối dựa trên ip và port -> Nhanh
Layer 7 (Application): Điều phối dựa trên tính toán

## Important functions

Health check
Sticky session: giữ người dùng tại 1 server cố định tránh reset network connection.
TLS/SSL: Xử lý internet security, giúp internal bỏ qua security giúp tăng performance.

## Vị trí trong architecture

Client -> LB -> API Gateway: chia tải cho cụm backend
LB -> Service: chia tải cho cụm backend
LB -> Database: Chia tải cho cụm Database (replica)
