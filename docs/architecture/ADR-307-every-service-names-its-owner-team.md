---
name: ADR-307
description: Every service names its owner team in its manifest.
globs: []
alwaysApply: true
kind: adr
status: Accepted
owner: Platform architect
revision: 1
origin: https://shl.atlassian.net/wiki/spaces/~71202058c67a3fe34f4165a8b8c49dc509cd2f/pages/474876489/ADR-307+Every+service+names+its+owner+team+in+its+manifest (Confluence page 474876489)
---

# ADR-307: Every service names its owner team in its manifest

## Decision

Every service names its owner team in its manifest.

## Rules

- MUST use Postgres for a new relational store.
- MUST NOT share a schema between two domains.

## Why

Each team picks its own database today.

## Rejected

- Not keep every engine, because three sets of backups to run.
- Not MySQL everywhere, because no team runs it today.

## Accepted costs

- One backup design for the estate.
