---
name: ADR-103
description: Every HTTP API at Larder names its major version in the media type of the Accept header, and never in the path.
globs: ["**/*"]
alwaysApply: false
kind: adr
status: Accepted
owner: Global Architecture Board
revision: v1, 2026-10-21
origin: the Larder Global Architecture Board minutes of 2026-10-21, item 3
---

# ADR-103: The API version is in the media type

## Decision

A client asks for a major version of an API in the `Accept` header. The path names the resource only.

## Rules

- MUST read the major version from the media type of the `Accept` header, as `application/vnd.larder.v1+json`.
- MUST NOT put a version in a path, such as `/v1/recipes`.
- MUST answer 406 when the `Accept` header asks for a major version the API does not serve.

## Why

A path with a version changes every link and every cache key at each new major version. Source:
the Board minutes of 2026-10-21, item 3.

## Rejected

- Not the version in the path, because the gateway caches by path.

## Accepted costs

- A person who tries an API in a browser sets the `Accept` header by hand.
