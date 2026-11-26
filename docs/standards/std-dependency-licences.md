---
name: std-dependency-licences
description: Which licences a runtime dependency of any Larder product may carry, and how a design records them.
globs: ["package.json", "composer.json", "go.mod", "pom.xml"]
alwaysApply: false
kind: standard
status: Active
owner: Global Architecture Board
revision: v1, 2026-10-21
origin: the Larder legal team's open-source policy, section OSS
---

# std-dependency-licences: Licences of runtime dependencies

## Applies to

Every runtime dependency of every product, in any language. It does not cover a dependency used only
to build or test, which never ships.

## Must

- **std-dependency-licences.01** MUST NOT add a runtime dependency under a licence other than MIT, Apache-2.0, BSD-2-Clause, BSD-3-Clause or ISC. *(Source: OSS-1)*
- **std-dependency-licences.02** MUST name the licence of each new runtime dependency in the design that adds it. *(Source: OSS-2)*

## Should

None.

## Values

| Value | Setting | Applies to | Source |
|---|---|---|---|
| The licences a runtime dependency may carry | MIT, Apache-2.0, BSD-2-Clause, BSD-3-Clause, ISC | std-dependency-licences.01 | OSS-1 |

## Checked by

- **std-dependency-licences.01** — review: the design names the licence of each new runtime dependency.
- **std-dependency-licences.02** — review: the critic reads the licence the design names.

## Relations

- A product's own standard may allow fewer licences, inside std-dependency-licences.01.

## Exceptions

The Global Architecture Board grants an exception in `docs/exceptions/`.

## Unresolved

None.

## Origin

The Larder legal team's open-source policy, section OSS, of 2026-10-21.
