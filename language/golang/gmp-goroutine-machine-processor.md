---
tags:
  - golang
  - goroutine
  - concurrency
---

# GMP Model

G - Goroutine: is a unit to execute tasks.
M - Machine: provide thread, hook to a target P.
P - Processor: manage G and assign to M to execute. 
global run queue: manage G if not assigned
local run queue: hook to P, in concurrency, 1 P = 1 local run queue = 1 thread

## Design

Program create a new G. G will be sent to local run queue of P (if not available, sent to global run queue).
P will assign G to a thread (in M) and execute.
##### Timeout handling
if a G is timeout (10ms), **sysmon** (system monitor) will pause this G. P will leave the current thread and request a new one from M (call it **Hand-off**).
if G is a network task -> move G to network pooler. (special case bacause of os epoll/kqueue)

after paused, G will be assign to global run queue and will be re-assign it to P.

If a P is free (local run queue is empty), get G from (global run queue, local run queue of other P **work-stealing**, network pooler)

## Related
[[goroutine]]
[[go-channel]]