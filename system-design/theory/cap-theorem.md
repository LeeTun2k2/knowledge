---
tags:
  - system-design
  - cap-theorem
---
# CAP

In distributed system, we can only ensure 2/3 keys:
- Consistency -> All request at a same time return a same result
- Availability -> Server is always available. User can always read and write.
- Partition Tolerence -> System can operate despire or failure.

## Specific system

### CA

Can reach if all service in on a same host.

## CP

Sacrifice availability during network failure.

## CA

Quickly response but only sync data at eventual consistency.

