---
name: std-eng-go
description: How every public API names its eng-go.
globs: ["**/*.go"]
alwaysApply: false
kind: standard
status: Active
owner: Lead architect
revision: 1
origin: https://shl.atlassian.net/wiki/spaces/~71202058c67a3fe34f4165a8b8c49dc509cd2f/pages/474679650/std-eng-go+Go+services (Confluence page 474679650)
---

# std-eng-go: Go services

## Applies to

Every public HTTP API. Internal events are not covered.

## Must

- **std-eng-go.01** MUST use kebab-case in a path segment.
- **std-eng-go.02** MUST NOT put a verb in a path.

## Should

- **std-eng-go.03** SHOULD keep a path under five segments.

## Values

| Value | Setting | Applies to | Source |
| --- | --- | --- | --- |
| Path segments | 5 at most | All public APIs | API guide, page 2 |

## Checked by

- std-eng-go.01 — review: the reviewer reads every new path.

## Relations

None.

## Exceptions

- Granted by: Lead architect

## Unresolved

None.

## Origin

- Source: API guide v2, the API guild space
