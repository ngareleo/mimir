# Architecture

The architecture domain captures the technical decisions taken for the Mimir rebuild before code is written. It documents *what is true and why* — the stack, the contracts, the conventions — so future engineers and AI agents can ground themselves quickly. Product behaviour lives in `docs/product/index.md`; visual structure lives in `docs/design/index.md`.

## Direct children

- [00-overview.md](00-overview.md) — system shape, request lifecycle, and how the pieces fit.
- [01-stack.md](01-stack.md) — chosen technologies with one-line rationale per layer.
- [02-repo-structure.md](02-repo-structure.md) — monorepo layout: `apps/*` + `packages/*`.
- [03-client/](03-client/index.md) — Flutter client: state, routing, platforms, codegen.
- [04-network-and-graphql/](04-network-and-graphql/index.md) — Ferry, the SDL contract, REST exceptions, tracing.
- [05-server/](05-server/index.md) — Rust + Axum + async-graphql server: layout, bins, errors.
- [06-database/](06-database/index.md) — Neo4j AuraDB: rationale, tiers, migrations, constraints.
- [07-data-model/](07-data-model/index.md) — node labels, relationships, per-entity shape.
- [08-authentication/](08-authentication/index.md) — Google OAuth, email/password, JWT, Apple-deferred.
- [09-notifications/](09-notifications/index.md) — local vs FCM, scheduling, cache bridge.
- [10-offline-and-sync/](10-offline-and-sync/index.md) — cache reads, online-only writes, reconciliation.
- [11-sharing-and-permissions/](11-sharing-and-permissions/index.md) — ownership, moderators, approvals, fan-out.
- [12-telemetry/](12-telemetry/index.md) — OTel on server, Sentry on client, trace correlation.
- [13-file-storage/](13-file-storage/index.md) — Tigris pre-signed uploads.
- [14-deployment/](14-deployment/index.md) — Fly.io, AuraDB, CI/CD, secrets.
- [15-testing/](15-testing/index.md) — client widget/golden tests, server cargo tests.
- [16-graphql-conventions/](16-graphql-conventions/index.md) — pagination, errors, naming, IDs.
