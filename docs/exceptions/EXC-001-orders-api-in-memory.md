---
name: EXC-001
description: pg-yassinesabri-shl-orders-api keeps its orders in memory in one process, because the playground runs it without a store.
kind: exception
hubs: ["SHOP", "OPS"]
repositories: ["pg-yassinesabri-shl-orders-api"]
departs: ["ADR-104"]
status: Active
review-by: 2027-10-21
owner: pg-yassinesabri-shl-orders-api tech lead
approver: Global Architecture Board
revision: v1
origin: this repository
---

# EXC-001 — pg-yassinesabri-shl-orders-api keeps its orders in memory

## Reason

ADR-104 requires every service to keep its state in a durable store. `pg-yassinesabri-shl-orders-api` keeps its orders
and its idempotency keys in the memory of one process, and a restart loses both. The playground
runs it with no database, so that a person can start it with `go run` and nothing else.

## Scope

This exception covers only the orders and the idempotency keys of `pg-yassinesabri-shl-orders-api`. It does not cover a
new kind of state that `pg-yassinesabri-shl-orders-api` adds, or any other service.

## Alternatives considered

1. **An embedded database file.** Rejected. A file on disk survives a restart in one place only,
   and the playground starts a new copy for each test.
2. **A database in a container.** Rejected. It needs a container runtime on the machine of every
   person who runs the playground.

## What the product does instead

`pg-yassinesabri-shl-orders-api`'s own record ADR-001 states the decision: one process, one lock over every state
change, and a restart loses every order.

## Exit

This exception ends when `pg-yassinesabri-shl-orders-api` gains a store. ADR-104 then binds it in full.
