This file covers the database migration workflow.

## Storage

Migration files live at `apps/server/migrations/*.cypher`, one file per migration. Filenames are prefixed with a zero-padded sequence number and a short description:

```
apps/server/migrations/
  0001_initial_constraints.cypher
  0002_add_shareable_link.cypher
  …
```

Files contain one or more Cypher statements. Each file is applied as a unit; partial application is not supported.

## Runner

The `migrate` cargo binary (see `docs/architecture/05-server/02-cargo-bins.md`) reads the files in lexicographic order, compares them against a list of applied migrations stored on a singleton `:Schema` node, and runs any that have not yet been applied. Each successful application updates the singleton inside the same transaction.

The binary is idempotent: running it twice in a row is a no-op on the second call.

## Production trigger

The `migrate` binary is configured as Fly.io's `release_command`. Fly runs the release command exactly once per deploy, *before* any machine is flipped to the new image. This gives us two guarantees:

1. The new app version never starts against an un-migrated database.
2. There is no cross-machine race: only one release-command instance runs per deploy.

If the migration fails, the deploy is aborted and the old machines continue serving. The failure surfaces in Fly logs and in OTel error events; see `docs/architecture/12-telemetry/01-server-otel.md`.

## Local and staging

Engineers run `cargo run --bin migrate` against their AuraDB Free instance manually. The same binary, the same migration files; only the connection URI differs (loaded from env). Staging is migrated by re-deploying — its `release_command` fires the same way.

## Rollbacks

There is no automated rollback. A bad migration is fixed by writing a new migration that compensates it. The `:Schema` singleton keeps the audit trail.
