This file covers the overall shape of the network surface between client and server.

## Surface

- **GraphQL-first.** Every domain operation — queries, mutations — is GraphQL. The server is built with `async-graphql`; the client uses Ferry.
- **REST exceptions.** Four endpoints exist outside GraphQL because GraphQL cannot serve them: `GET /auth/google/callback`, `GET /auth/reset?token=…`, `GET /healthz`, `GET /metrics`. Detail in `docs/architecture/04-network-and-graphql/03-rest-exceptions.md`. New REST endpoints require a documentation update.
- **No subscriptions in v1.** Real-time updates ride on FCM data messages that trigger Ferry cache invalidation. The schema is modelled so subscriptions can be added later without breaking changes. See `docs/architecture/09-notifications/03-push-to-cache-bridge.md`.

## Cross-cutting headers

Every GraphQL request carries:

- `Authorization: Bearer <access-token>` — JWT issued by `/auth/*` flows. See `docs/architecture/08-authentication/03-jwt.md`.
- `traceparent: <W3C trace context>` — injected by a Ferry link, propagated into OTel spans on the server. See `docs/architecture/04-network-and-graphql/04-trace-propagation.md`.

## Contract management

The SDL lives at `packages/graphql-schema/schema.graphql`. It is *generated*, not authored: the server's resolvers are the source of truth. A pre-commit hook writes the SDL; CI fails on drift. Both apps consume the same file.

## Errors and pagination

GraphQL conventions — error shape, pagination, naming, ID strategy — live in `docs/architecture/16-graphql-conventions/index.md`. They apply uniformly to every operation.
