# Testing

This folder covers Mimir's test strategy across the client and server.

The client uses widget tests for individual screens and golden tests for visual-regression coverage of the structural mockups. The server uses `cargo test` against a shared dev Neo4j instance with per-test isolation through namespaced node creation. No integration tests across the Flutter↔Rust boundary in v1 — those are deferred to a future end-to-end harness once the app is in user hands.

## Direct children

- [00-overview.md](00-overview.md) — high-level strategy and what it deliberately doesn't test.
- [01-client-widget-and-golden.md](01-client-widget-and-golden.md) — Flutter widget tests, golden tests, mock data conventions.
- [02-server-cargo-and-neo4j.md](02-server-cargo-and-neo4j.md) — server unit and integration tests, shared dev Neo4j with per-test isolation.
