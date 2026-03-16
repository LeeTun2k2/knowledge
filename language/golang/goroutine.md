---
tags:
  - golang
  - goroutine
---
# Goroutine

Is a execution unit in go.

## Pros

Init size 2KB << 1MB of thread
manage by go scheduling -> reduce blocking

## How it work

Create a goroutine -> Assign to local run queue (if queue is overflow, assign to global run queue)
-> Processor collect goroutine to local queue.
-> Processor pop and assign to thread to execute.

## Life cycle

if main function is closed, all created goroutines will be disposed.

## Related
[[go-concurrency]]
[[gmp-goroutine-machine-processor]]
[[thread]]