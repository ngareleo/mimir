This file covers the monorepo layout and per-folder responsibility.

## Layout

```
apps/
  client/      # Flutter (web + iOS + Android)
  server/      # Rust + Axum + async-graphql + neo4rs
    src/
    migrations/        # *.cypher files applied by the migrate binary
    bin/
      migrate.rs       # cargo run --bin migrate
      print-schema.rs  # cargo run --bin print-schema
packages/
  graphql-schema/      # committed SDL output of print-schema
docs/                  # this tree
```

## Why a monorepo

- The GraphQL SDL is the cross-cutting contract; one repo lets a pre-commit hook in `apps/server` write into `packages/graphql-schema` atomically. See `docs/architecture/04-network-and-graphql/02-schema-source-of-truth.md`.
- Client codegen consumes `packages/graphql-schema/schema.graphql` directly — no publishing step.
- CI runs lint + tests for both apps and the SDL-drift check in a single pipeline.

## Per-folder responsibility

- **`apps/client`** — all Flutter code. Owns its own `pubspec.yaml`, codegen output, tests, and platform shells. Reads the SDL from `packages/graphql-schema`.
- **`apps/server`** — all Rust code. The library crate hosts resolvers, persistence, auth, telemetry. Two cargo binaries live under `bin/`: `migrate` (applies Cypher migrations) and `print-schema` (writes the SDL). See `docs/architecture/05-server/02-cargo-bins.md`.
- **`packages/graphql-schema`** — committed `schema.graphql`. Read-only for humans; produced by the pre-commit hook; verified in CI for drift.
- **`docs/`** — this tree. Product, design, and architecture domains.

New runnable surfaces (additional apps, additional packages) are added by extending this layout, not by creating sibling top-level folders.
