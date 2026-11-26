# pg-yassinesabri-shl-global-context

The playground's global context repository. It holds the decisions and standards that bind every
Larder product, and the exceptions that the Global Architecture Board approved. It holds no
requirement, no design and no code. `make playground-up` pushes it with the release tag `v1.0.0`.

| Id | What it is | Binds |
|---|---|---|
| `global.ADR-101` | One request id per call, `x-request-id` | Always on: every design reads it |
| `global.ADR-102` | Every time is UTC | Every path |
| `global.ADR-103` | The API version is in the media type, never in the path | Every path |
| `global.ADR-104` | State survives a restart | Every path |
| `global.std-dependency-licences` | Which licences a runtime dependency may carry | `package.json`, `composer.json`, `go.mod`, `pom.xml` |
| `global.EXC-001` | `pg-yassinesabri-shl-orders-api` keeps its orders in memory | The hubs `SHOP` and `OPS`, in `pg-yassinesabri-shl-orders-api` only |

The two hubs start with no link to this repository. `examples/playground/README.md` says how to
connect each one with `/check-global --connect`, and what the check finds.
