This file covers server-side tests in Rust.

## Test layout

- `apps/server/src/**/*` with `#[cfg(test)] mod tests` — unit tests, no Neo4j.
- `apps/server/tests/*.rs` — integration tests against a real Neo4j. Each integration test file gets its own `mod` and shares a `setup()` helper.

## Shared Neo4j

In CI, a `neo4j:5-community` service container starts at the beginning of the workflow and stays up for the whole run. Locally, developers run `docker compose up neo4j-test` from `apps/server/`.

## Per-test isolation

The server's data model uses a shared Neo4j instance — there is no per-test database. Isolation is achieved through:

1. Each test generates a unique `runId` (UUIDv7) at the top of `setup()`.
2. All test data is created with a `:TestRun { runId }` marker and `[:CREATED_BY_TEST]` relationships back to it.
3. Queries inside the test scope through `runId` to read only this test's nodes.
4. `tearDown()` deletes everything connected to the `:TestRun` node by traversing `[:CREATED_BY_TEST*0..]`.

Parallel test execution is safe because every test reads and writes within its own `runId` subgraph.

## Resolver tests

Each GraphQL resolver has at least one test that runs the operation through `async_graphql::Schema::execute`, asserts on the JSON response, and verifies side effects in Neo4j. Tests use a shared `Schema` instance built once per integration test binary.

## FCM and Tigris

Both are abstracted behind traits (`FcmClient`, `ObjectStore`). Tests use in-memory fake implementations that record calls; no real network IO.
