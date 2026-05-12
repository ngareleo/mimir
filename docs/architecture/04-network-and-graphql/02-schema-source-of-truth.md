This file covers how the GraphQL SDL is produced, committed, and verified.

## Source of truth

Server resolvers are the source of truth for the schema. The SDL is *generated* — never hand-edited.

## Mechanism

The server hosts a cargo binary, `print-schema`, that boots the `async-graphql` schema with the same configuration the runtime uses and writes its SDL to `packages/graphql-schema/schema.graphql`. See `docs/architecture/05-server/02-cargo-bins.md`.

```
cargo run --bin print-schema
```

## Pre-commit hook

A repo-level pre-commit hook runs the binary before any commit that touches `apps/server/src/**`. The hook writes the SDL into `packages/graphql-schema/schema.graphql` and stages it. Contributors do not edit the SDL by hand; if the hook is bypassed, CI catches the drift.

## CI drift check

A dedicated CI job runs `print-schema` on every pull request, then diffs the output against the committed file. Non-zero diff fails the build. This guarantees the SDL committed in `packages/graphql-schema` always matches the resolvers in `apps/server`.

## Why this matters

- The client codegen reads the committed SDL. Drift would silently produce stale types.
- Reviewers can read a single file (`schema.graphql`) to see exactly what the server exposes, without running anything.
- The contract is reviewable in PRs: schema changes show up as diffs in `packages/graphql-schema/schema.graphql`.

## What this is not

- The committed SDL is not a separate spec the server must match. The opposite is true: the resolvers define the schema, and the file is downstream.
- There is no schema registry, no schema linting service, no federation gateway in v1.
