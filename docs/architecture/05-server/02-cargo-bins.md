This file covers the two auxiliary cargo binaries that ship with the server.

## `migrate`

Applies Cypher migrations against the configured Neo4j database.

- **Source**: `apps/server/src/bin/migrate.rs`.
- **Input**: `apps/server/migrations/*.cypher`, in lexicographic order. See `docs/architecture/06-database/02-migrations.md`.
- **Run**: `cargo run --bin migrate`.
- **Production trigger**: configured as Fly.io's `release_command`. Fly runs it once per deploy, before machines are flipped to the new version. Serial by construction — no inter-machine race.
- **Dev trigger**: run by hand against the dev AuraDB Free instance.
- **State**: the binary records applied migrations on a singleton `:Schema` node. Re-running is idempotent.

## `print-schema`

Emits the canonical SDL produced by the server's `async-graphql` schema.

- **Source**: `apps/server/src/bin/print-schema.rs`.
- **Output**: written to `packages/graphql-schema/schema.graphql`.
- **Run**: `cargo run --bin print-schema`.
- **Pre-commit**: a repo hook runs it when commits touch `apps/server/src/**` and stages the result. See `docs/architecture/04-network-and-graphql/02-schema-source-of-truth.md`.
- **CI**: the SDL-drift job runs the binary, diffs the result against the committed file, and fails the build on non-zero diff.

## Why these are binaries, not subcommands

Both run in isolation from the HTTP server — `migrate` runs as a release step, `print-schema` runs in CI and locally without booting any networking. A binary keeps them independent of the server's startup path. Each has a minimal `main` that reuses the library's `config` and (for `migrate`) `db` modules; see `docs/architecture/05-server/01-module-layout.md`.
