---
tags:
  - database
---
# What happen when we write a super large record to database

## Problem

Page size is 4kb or 8kb
What happen if we write 1MB data to database?

## What happen

Write all data to multi page (call it overflow page)
Store address of all used pages 

## Causing

Require multi-step to access data -> get SLOW

## Solution

Compression.
External / Object storage.
Vertical Partitioning

## Related 

[[split-page]]