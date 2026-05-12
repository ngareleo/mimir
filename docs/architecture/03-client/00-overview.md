This file covers how the Flutter client is layered and what each layer owns.

## Layering

The client splits into four concerns that hold across all three targets.

1. **Presentation** — widgets composed with `flutter_hooks`. No direct I/O. Reads providers, dispatches mutations.
2. **State** — `hooks_riverpod` providers. Holds derived state, auth status, in-flight mutation status, and exposes Ferry query results.
3. **Network** — a single Ferry `Client` instance with a Hive-backed normalised cache and a link chain that injects auth and the W3C `traceparent`. Detail in `docs/architecture/04-network-and-graphql/01-ferry-client.md`.
4. **Local services** — notifications, secure storage, the offline-write gate. These are providers that wrap platform plugins behind a Dart interface.

## What the client owns end-to-end

- The Ferry persisted cache (read source of truth when offline).
- The `scheduled_notifications` Hive box (schedule source of truth, independent of Ferry cache). See `docs/architecture/09-notifications/01-local-scheduling.md`.
- The Sentry SDK, tagged with the same trace ID it sends on the wire. See `docs/architecture/12-telemetry/03-trace-correlation.md`.
- A clearly-rendered offline state any time a mutation cannot run; writes are not queued. See `docs/architecture/10-offline-and-sync/02-write-policy.md`.

## What the client does not own

- Subscriptions or any long-lived GraphQL connection — v1 is request/response plus FCM data messages.
- A REST client library — the small REST exception list is hit with `package:http` directly.
- Schema authoring — the SDL is read from `packages/graphql-schema`; the client never edits it.
