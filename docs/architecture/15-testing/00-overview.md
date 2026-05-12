This file covers the overall test strategy and its boundaries.

## What we test

- **Client widget tests:** rendering and interaction of individual screens, with Ferry replaced by a hand-stubbed result map.
- **Client golden tests:** key screens (home, semester tabs, timetable view) rendered to PNG and compared against committed reference images. Regenerated only on intentional UI changes via `flutter test --update-goldens`.
- **Server unit tests:** pure-Rust logic (auth token rotation, schedule-window math, validation) with no Neo4j.
- **Server integration tests:** GraphQL resolvers exercised against a real Neo4j over Bolt. A shared dev instance is namespaced per test; see [02-server-cargo-and-neo4j.md](02-server-cargo-and-neo4j.md).

## What we deliberately do NOT test in v1

- **End-to-end (client → server → DB).** Deferred until the app has users; the cost-to-coverage ratio is too low pre-launch.
- **Push delivery.** FCM is mocked in server tests; we trust Google's transport.
- **Tigris uploads.** Pre-signed URL generation is unit-tested; actual PUTs to Tigris are not asserted in CI.
- **Flutter integration tests against a simulator.** Patrol or similar tools are deferred; widget + golden coverage is enough at this stage.

## Where tests run

- **Local:** `cargo test` and `flutter test` against any local Neo4j (Docker compose file in `apps/server/`).
- **CI:** GitHub Actions service container for Neo4j, fresh per workflow run. See `docs/architecture/14-deployment/03-ci-cd.md`.

## Test data

Seed scripts in `apps/server/tests/seed/` create canonical fixtures (sample semester, two users, a shared resource). Tests reuse them by namespacing reads with a per-test `:TestRun { runId }` marker.
