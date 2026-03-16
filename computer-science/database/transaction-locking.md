---
tags:
  - sql
  - database
---
# Transaction locking

On concurrency control, it should have rules to manage data access right.

## Locking

Read => **Shared Lock (S)** => Can be shared
Write => Exclusive Lock (X) => Can not be shared

## Multi-Version Concurrency Control

Instead of locking, db engineer create a snapshot of data
=> Improve read performance.
