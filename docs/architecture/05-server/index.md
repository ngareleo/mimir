# Server

The server is a Rust + Axum service exposing an `async-graphql` schema backed by Neo4j via `neo4rs`. It runs on Fly.io in the `jnb` region. The same binary serves the GraphQL endpoint and the small REST exception list documented in `docs/architecture/04-network-and-graphql/03-rest-exceptions.md`. Two auxiliary cargo binaries — `migrate` and `print-schema` — ship in the same crate.

## Direct children

- [00-overview.md](00-overview.md) — what the server is responsible for and how requests are processed.
- [01-module-layout.md](01-module-layout.md) — crate structure and module boundaries.
- [02-cargo-bins.md](02-cargo-bins.md) — the `migrate` and `print-schema` binaries.
- [03-error-handling.md](03-error-handling.md) — the `async-graphql` error contract.
