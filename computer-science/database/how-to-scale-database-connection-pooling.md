---
tags:
  - database
---
# How to scale database connection pooling

## Problem

Max connection should be: CPU * 2.
If a lot of application connect to a database and require a huge connections, most of logical connection will be getting timeout and throw exception.

Then, how to scale database to receive a huge of connection?
## Solution

##### Connection Proxy (as a Load Balance)

Hold logical connection in a proxy => will protect database and prevent from burst of traffics.
=> Delay only, do not resolve root cause.
##### Vertical scale 

Increase number of CPU and RAM. 
=> Downtime to upgrade server

##### Read Replica (Horizontal scale)

Add more servers, each server is a replica.

=> Reduce consistency. 

## Related

[[database-architecture]]