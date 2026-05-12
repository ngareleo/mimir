This file covers the high-level shape of the Mimir system and the lifecycle of a typical request.

## System shape

Three runtime pieces, one shared contract.

- **Client** — a single Flutter codebase running on web, iOS, and Android. Talks to the server over GraphQL via Ferry, with a small documented REST surface for OAuth callbacks, password-reset landing, health, and metrics.
- **Server** — a Rust + Axum service in Fly.io `jnb` exposing `async-graphql` resolvers backed by `neo4rs`. The same binary serves the REST exceptions.
- **Database** — Neo4j AuraDB. Free tier for dev/staging; Pro Starter in `eu-west` for prod. Server-to-DB hop sits at ~150ms.

The shared contract is the GraphQL SDL in `packages/graphql-schema`, generated from the server's resolvers. See `docs/architecture/04-network-and-graphql/02-schema-source-of-truth.md`.

## Request lifecycle

A typical read:

1. Client issues a GraphQL operation through Ferry. The Ferry link injects a W3C `traceparent` and a JWT access token.
2. Server validates the JWT, opens an OTel span tied to the incoming `traceparent`, runs the resolver, and returns the response.
3. Ferry writes the result into the Hive-backed normalised cache.

A typical write that touches shared data:

1. Mutation runs on the server, updates Neo4j, returns the new state.
2. Server fans out FCM data-only pushes to affected users via `tokio::spawn` (best-effort).
3. Receiving clients invalidate the affected Ferry query, refetch, and reconcile their local notification schedule.

## Supporting flows

- File uploads use pre-signed Tigris URLs returned from `requestUpload` / `completeUpload` mutations.
- Crashes and breadcrumbs go to Sentry; server traces and metrics go to Axiom. The W3C trace ID is the join key.
