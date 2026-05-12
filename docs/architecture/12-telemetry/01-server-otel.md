This file covers OpenTelemetry on the server.

## SDK

The server uses the Rust OpenTelemetry SDK with the OTLP exporter targeting Axiom over HTTPS. Configuration is loaded at startup from environment variables (Axiom endpoint, API token, service name, environment tag).

## What gets a span

- **Every HTTP request.** Tower middleware opens a span whose name is the route (e.g. `POST /graphql`) and whose parent is the incoming `traceparent`. See `docs/architecture/04-network-and-graphql/04-trace-propagation.md`.
- **Every GraphQL operation.** A child span inside the HTTP request, named with the operation kind and name. Attributes: operation name, authenticated user ID (if any), variable count.
- **Every Cypher query.** A child span around the `neo4rs` call. Attributes: query template (parametrised, never with inlined values), result row count.
- **Every outbound HTTP call.** Wrapped in a span via the `reqwest`-OTel integration.
- **Spawned fan-out tasks.** Each FCM send is its own span; the span lives outside the request because the request has already returned. The fan-out's parent context is captured at spawn time so the spans link back to the originating request.

## Attributes and naming

- Service name: `mimir-server`.
- Environment: `development` | `staging` | `production`.
- Trace IDs: 16 bytes, hex-encoded, sourced from the inbound `traceparent` when present.
- Span names: stable strings, never including user-supplied data.

## Metrics

The Prometheus-style endpoint at `/metrics` exposes:

- HTTP request count and duration histograms per route.
- GraphQL operation count and duration histograms per operation name.
- Neo4j connection pool gauges.
- FCM send success and failure counters.

## Logs

Structured logs use `tracing` and are emitted both to stdout (collected by Fly) and as OTel log records (collected by Axiom). Log levels above `INFO` are exported; `DEBUG` is local-only.

## What this is not

Not a profiler, not an APM agent, not a service mesh. OTel is the framing; Axiom is the backend; everything else falls out of those two choices.
