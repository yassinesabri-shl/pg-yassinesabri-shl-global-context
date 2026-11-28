---
name: std-sec-auth
description: How every public API names its sec-auth.
globs: []
alwaysApply: false
kind: standard
status: Deprecated
owner: Lead architect
revision: 3
origin: https://shl.atlassian.net/wiki/spaces/~71202058c67a3fe34f4165a8b8c49dc509cd2f/pages/474678614/std-sec-auth+Authentication (Confluence page 474678614)
---

# std-sec-auth: Authentication

## Applies to

Every public HTTP API. Internal events are not covered.

## Must

- **std-sec-auth.01** MUST use kebab-case in a path segment.
- **std-sec-auth.02** MUST NOT put a verb in a path.

## Should

- **std-sec-auth.03** SHOULD keep a path under five segments.

## Values

| Value | Setting | Applies to | Source |
| --- | --- | --- | --- |
| Path segments | 5 at most | All public APIs | API guide, page 2 |

## Checked by

- std-sec-auth.01 — review: the reviewer reads every new path.

## Relations

None.

## Exceptions

- Granted by: Lead architect

## Unresolved

None.

## Origin

- Source: API guide v2, the API guild space
