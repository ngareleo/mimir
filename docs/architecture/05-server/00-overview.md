This file covers what the server is responsible for and how a request is processed.

## Responsibilities

- Serve the GraphQL endpoint at `/graphql` for all domain operations.
- Serve the documented REST exceptions: `/auth/google/callback`, `/auth/reset`, `/healthz`, `/metrics`.
- Issue and rotate JWT access and refresh tokens. See `docs/architecture/08-authentication/03-jwt.md`.
- Persist and read domain data through `neo4rs` against AuraDB.
- Fan out FCM data-only push messages for shared-resource changes. See `docs/architecture/11-sharing-and-permissions/04-fan-out.md`.
- Return pre-signed Tigris URLs for uploads. See `docs/architecture/13-file-storage/01-presigned-upload-flow.md`.
- Emit OpenTelemetry spans and metrics to Axiom. See `docs/architecture/12-telemetry/01-server-otel.md`.

## Request lifecycle

A GraphQL request:

1. **Tower middleware** logs the request, opens an OTel span tied to the incoming W3C `traceparent`, and runs the auth extractor.
2. **Auth extractor** validates the `Authorization: Bearer …` JWT and adds the resolved user identity to the request context. Missing or invalid tokens still pass for public operations; resolvers gate themselves.
3. **`async-graphql` executor** runs the resolvers, sharing a `neo4rs` connection pool and the user identity through `Context`.
4. **Side effects** (FCM fan-out, upload-state writes) are dispatched from within resolvers as `tokio::spawn` tasks where the work is best-effort.
5. **Response** is serialised back through Axum.

## What the server does not do

- Hold long-lived connections to clients. No GraphQL subscriptions, no WebSockets in v1.
- Cache responses. The Ferry persisted cache on the client is the response cache.
- Schedule notifications. The client owns its `scheduled_notifications` box.
