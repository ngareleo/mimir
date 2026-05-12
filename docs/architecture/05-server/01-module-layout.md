This file covers the Rust crate's module boundaries.

## Top-level modules

The library crate splits along the seams of the system rather than by technical layer.

- **`http`** — Axum routing, middleware (auth, tracing, CORS), and the REST exception handlers.
- **`graphql`** — schema construction, root types, custom scalars, and the `Context` shape. Resolvers live in domain submodules under here.
- **`domain`** — pure types for entities (`User`, `Semester`, `Unit`, `Lecture`, `Timetable`, `Task`, `Event`, `Assignment`, `Paper`, sharing primitives). No I/O. Mirrors the labels in `docs/architecture/07-data-model/index.md`.
- **`db`** — `neo4rs` access. One submodule per entity grouping. All Cypher lives here; resolvers call functions, not Cypher strings.
- **`auth`** — JWT issuing/verifying, Google OAuth code exchange, password-reset token mint and verify.
- **`notify`** — FCM client and the fan-out helper that resolvers call. See `docs/architecture/11-sharing-and-permissions/04-fan-out.md`.
- **`storage`** — Tigris client; pre-signed URL helpers.
- **`telemetry`** — OTel SDK initialisation and the `traceparent` parsing helper.
- **`config`** — environment variable loading; one struct per concern (DB, Auth, FCM, Tigris, OTel).

## Conventions

- Resolvers depend on `db` and `auth` and `notify` through trait objects in `Context` where it improves testability; otherwise they call free functions directly.
- `domain` types are the shared vocabulary. GraphQL output types and `db` row types both convert into them.
- Cypher strings are not inline in resolvers. They live in `db` next to the function that runs them.
- Side-effecting `tokio::spawn` calls live in `notify` (fan-out) and `storage` (upload finalisation), never inline in resolvers.

## Binary crates

The two cargo binaries — `migrate` and `print-schema` — depend on the library and reuse `config`, `db`, and `graphql` as needed. See `docs/architecture/05-server/02-cargo-bins.md`.
