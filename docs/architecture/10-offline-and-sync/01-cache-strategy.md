This file covers the Ferry persisted cache strategy.

## Storage

The Ferry normalised cache is persisted to a Hive box. Hive works on every Mimir target: file storage on mobile, IndexedDB on web. The cache survives app restarts and offline periods.

## Normalisation

Ferry normalises by GraphQL type and ID. Every node returned from a query is keyed by its label and `id`; queries store a list of references to those keys. A mutation that returns the updated node updates *every* query that references the node — no manual cache patching needed in the common case.

This is the main reason Ferry was chosen over hand-rolled HTTP + manual caching: the typical resource update propagates without explicit cache writes from the call site.

## Read behaviour

By default, Mimir queries use a cache-first policy: serve cached data immediately, then refetch in the background and update if newer. Queries that must be fresh (e.g. inside the auth flow) opt into network-only.

## Eviction

Hive has no built-in size cap. Mimir bounds the cache by:

- **Per-query staleness threshold.** Cached query results older than 30 days are evicted on next access.
- **Box size watchdog.** A periodic background task measures the Hive box size; above a soft threshold it evicts the oldest queries first.
- **Sign-out clears everything.** On sign-out the cache box is dropped entirely.

These bounds are policy, not contract; the user-visible behaviour is "stale data eventually goes away".

## What is NOT in this cache

- The `scheduled_notifications` Hive box — that lives in its own box and is the schedule source of truth. See `docs/architecture/09-notifications/01-local-scheduling.md`.
- Auth tokens — those live in platform secure storage.
- Sentry breadcrumbs — those live in the Sentry SDK's own buffer.

Keeping the read cache pure makes eviction reasoning straightforward.
