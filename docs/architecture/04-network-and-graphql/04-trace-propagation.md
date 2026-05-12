This file covers how the W3C `traceparent` header flows from client to server.

## What the client does

A Ferry link generates a fresh W3C `traceparent` header for every GraphQL operation and attaches it to the outgoing request. The same trace ID is captured in scope on the client and tagged onto any Sentry event emitted while that operation is in flight. The detail of Sentry-side tagging lives in `docs/architecture/12-telemetry/03-trace-correlation.md`.

The header conforms to the W3C Trace Context spec:

```
traceparent: 00-<32-hex-trace-id>-<16-hex-span-id>-01
```

The client does not run a full OTel SDK. It generates IDs locally and attaches them; no spans are emitted from the client.

## What the server does

The server's Axum middleware reads `traceparent`, parses it, and uses it as the parent context for the request's OTel span. All downstream spans (resolver work, `neo4rs` queries, outbound HTTP) hang off that root. Spans export to Axiom via OTLP. Setup detail in `docs/architecture/12-telemetry/01-server-otel.md`.

If the header is absent or malformed, the server starts a fresh trace and logs a warning. This should not happen from the Mimir client; it only occurs for tools poking the API directly.

## What this enables

- A Sentry crash on the client and the OTel trace for the operation that caused it share the trace ID.
- Server-side incident response can pivot from an Axiom span to the Sentry event for the same user interaction.
- Cross-stack debugging stops being archaeology.

## What this does not do

The REST exception endpoints do not propagate trace context — they have no client-side Ferry link. They start their own server-side traces.
