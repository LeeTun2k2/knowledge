---
tags:
  - idempotency
  - api-design
  - system-design
---
# Idempotency

## Key Ideas

Ensure 1 request only execute 1 time although a lot of same requests were sent.

## Implement

1. Create idempotency-key (ussually use UUID)
2. Attach idempotency-key into api header.
3. Always proceed if idempotency-key is valid.

## Related
