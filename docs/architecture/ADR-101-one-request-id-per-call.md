---
name: ADR-101
description: Every call at Larder carries one request id in the HTTP header x-request-id, set once by the first system and passed on unchanged.
globs: []
alwaysApply: true
kind: adr
status: Accepted
owner: Global Architecture Board
revision: v1, 2026-10-21
origin: the Larder Global Architecture Board minutes of 2026-10-21, item 1
---

# ADR-101: One request id names a call in every system it reaches

## Decision

Every call carries one request id in the HTTP header `x-request-id`, from the first system that sees the call to every system it reaches.

## Rules

- MUST use the `x-request-id` of an incoming request when it is well formed, and generate a UUID v4 when it is not.
- MUST send the id back in the `x-request-id` header of every response.
- MUST pass the id unchanged in every outgoing call the request causes.
- MUST NOT add a second correlation header, such as `correlation-id` or `traceparent`.

## Why

A support engineer follows one failed call through every product with one value. Source: the Board
minutes of 2026-10-21, item 1.

## Rejected

- Not a tracing header such as `traceparent`, because no product runs a tracing agent.

## Accepted costs

- A product that already sends another header changes it.
