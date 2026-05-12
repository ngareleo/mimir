This file covers the telemetry split between server and client.

## Two tools, two responsibilities

- **Server: OpenTelemetry → Axiom.** Spans, metrics, and structured logs from every request and side effect. The server is the system of record for what happened on the backend.
- **Client: Sentry.** Crashes and breadcrumbs from the Flutter app on every target. Captures what the user saw and what they did just before things broke.

The two are deliberately separate tools. Client crash insight (stack traces with source maps, user feedback, release tracking) is Sentry's strength. Server tracing and structured query (which spans took what time, which queries failed for which users) is Axiom's strength.

## What is captured server-side

- Spans for every GraphQL operation, with the operation name, the authenticated user (if any), and the duration.
- Spans for every Neo4j query, with the Cypher template and parameters truncated.
- Spans for every outbound HTTP call (Google OAuth verification, FCM send, Tigris pre-sign).
- Error events on the FCM fan-out path. See `docs/architecture/11-sharing-and-permissions/04-fan-out.md`.
- Liveness and pool metrics on `/metrics`.

## What is captured client-side

- Uncaught Dart exceptions and Flutter framework errors.
- Manual `Sentry.captureException` calls inside catch blocks where we want extra context.
- Breadcrumbs: navigations, GraphQL operations issued, notifications scheduled, sign-in events.
- Release versions, OS version, device class.

## The join

Every GraphQL request carries a W3C `traceparent` header. The server uses it as the OTel span parent. The client tags every Sentry event with the same trace ID. Detail in `docs/architecture/12-telemetry/03-trace-correlation.md`.

## What we deliberately do not capture

- PII beyond user ID. No emails in span attributes, no message bodies in breadcrumbs.
- Full request/response bodies. Query names and operation kinds only.
- Long-running session traces on the client. Sentry is for incidents, not analytics.
