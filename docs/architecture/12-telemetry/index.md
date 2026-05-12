# Telemetry

The server emits OpenTelemetry spans and metrics to Axiom. The client emits crash reports and breadcrumbs to Sentry. The two are joined by the W3C trace ID that travels on every GraphQL request. This folder documents each side and the bridge between them.

## Direct children

- [00-overview.md](00-overview.md) — the two-tool split and what each captures.
- [01-server-otel.md](01-server-otel.md) — OTel SDK on the server; Axiom exporter.
- [02-client-sentry.md](02-client-sentry.md) — Sentry on the client; breadcrumbs.
- [03-trace-correlation.md](03-trace-correlation.md) — how a Sentry event lines up with an Axiom trace.
