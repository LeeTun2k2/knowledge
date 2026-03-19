---
tags:
  - network
  - tcp
  - udp
---


| TCP (Transmission Control Protocol)             | UDP (User Datagram Protocol)           |
| ----------------------------------------------- | -------------------------------------- |
| Connection-oriented; uses a three-way handshake | Connectionless; no handshake           |
| Guarantees reliable data delivery               | Does not guarantee delivery            |
| Uses acknowledgements (ACKs)                    | No acknowledgements                    |
| Supports retransmission of lost packets         | No retransmission support              |
| Ensures packets are delivered in order          | Does not ensure ordering               |
| Provides flow control and congestion control    | No flow or congestion control          |
| Slower due to higher overhead                   | Faster with minimal overhead           |
| Variable header size (20–60 bytes)              | Fixed header size (8 bytes)            |
| Treats data as a continuous byte stream         | Treats data as independent messages    |
| Does not support broadcasting or multicasting   | Supports broadcasting and multicasting |
| Used by HTTP, HTTPS, FTP, SMTP                  | Used by DNS, DHCP, VoIP, Streaming     |

