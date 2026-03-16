---
tags:
  - golang
  - concurrency
  - goroutine
---
# Go concurrency

Proceed multi-task as individual units.

go **DO NOT use threads** directly, **USE goroutine**.

## Goroutine

A unit of go concurrency to execute task. 
smaller than thread, use for handle a huge of concurrent tasks

Thread -> init 1MB,
goroutine -> init 2kb

## Channels

is goroutine queue (pipe line).
has 2 types:
- buffer channel -> if reach limit, can not add more goroutine
- unbuffer channel -> can add unlimited goroutine

**“Do not communicate by sharing memory; instead, share memory by communicating.”**
**State có owner, interaction qua message.**

communicate by sharing memory: -> lock, semaphore, mutex, automic 
=> deadlock, nhiều thread tranh giành 1 lock gây tắt nghẽn, khó debug do không biết thread nào đã thay đổi variable

share memory by communicating ->  manage goroutine by channel
=> Biết owner của goroutine từ đâu ra để dễ trace. 
=> Channel kiểm soát goroutine nên giảm deadlock
## Select

Là event listener giúp lấy value ra từ channel. 
=> không cần pooling.
=> có thể chờ nhiều channel
=> có timeout, nonblocking

## How to await a goroutine

use wait.group (asame with Task.WhenAll in C#)

## Related
[[lock]]
[[semaphore]]
[[mutex]]
[[automic]]
