---
name: ADR-104
description: Every service at Larder keeps its state in a durable store, so that a restart or a crash loses no change it confirmed.
globs: ["**/*"]
alwaysApply: false
kind: adr
status: Accepted
owner: Global Architecture Board
revision: v1, 2026-10-21
origin: the Larder Global Architecture Board minutes of 2026-10-21, item 4
---

# ADR-104: State survives a restart

## Decision

A change that a service confirmed to its caller is in a durable store before the service answers.

## Rules

- MUST write a change to a durable store before the service confirms it.
- MUST NOT keep the only copy of a confirmed change in the memory of a process.

## Why

A deploy restarts every service, and a customer's paid order must still exist after it. Source:
the Board minutes of 2026-10-21, item 4.

## Rejected

- Not a replica in a second process, because both processes stop in one deploy.

## Accepted costs

- Every service runs, backs up and monitors a store.
