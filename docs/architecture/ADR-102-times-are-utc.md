---
name: ADR-102
description: Every time at Larder is stored, compared and sent in UTC as RFC 3339 with the suffix Z. A local time exists only on a screen or a printed label.
globs: ["**/*"]
alwaysApply: false
kind: adr
status: Accepted
owner: Global Architecture Board
revision: v1, 2026-10-21
origin: the Larder Global Architecture Board minutes of 2026-10-21, item 2
---

# ADR-102: Every time is UTC

## Decision

A time is UTC everywhere a system stores, compares or sends it.

## Rules

- MUST store, compare and send a time in UTC, as RFC 3339 with the suffix `Z`.
- MUST convert to a local time only to show it on a screen or print it on a label.

## Why

The warehouse and the shop run in two time zones, and a cut-off compared in two zones is wrong
twice a year. Source: the Board minutes of 2026-10-21, item 2.

## Rejected

- Not local time with an offset, because two offsets for one instant compare unequal as text.

## Accepted costs

- A person reads a stored time in UTC and converts it in their head.
