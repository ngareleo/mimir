This file covers regional latency tradeoffs.

## Server↔DB roundtrip ~150ms

- **What:** the Rust server in Fly `jnb` connects to AuraDB Pro in `gcp-europe-west1`. Every Bolt query pays ~150ms RTT.
- **Why:** AuraDB has no Africa region. The two alternatives — self-hosting Neo4j or moving the server to eu-west — each shift the latency elsewhere, and self-hosting trades away managed backups and HA.
- **Cost / mitigation:** a GraphQL operation that fans into N Cypher queries pays N × 150ms on the server side. We mitigate by designing operations to minimise round-trips: prefer a single richer Cypher query over many small ones, and lean on Ferry's persisted cache to keep warm reads off the server entirely. Documented in `docs/architecture/06-database/01-tiers-and-region.md`.

## Tigris has no Africa edge

- **What:** Tigris's edge network has no PoP in Africa as of this writing. First-byte latency from Kenya is ~200ms on cold reads.
- **Why:** few S3-compatible object stores have Africa edges; Tigris is the strongest Fly-native option.
- **Cost / mitigation:** uploads (the common case) are write-once, batched, and don't block the rendered UI. Reads are warmable; once a file is cached at an edge, subsequent fetches are fast. We don't attempt further mitigation in v1.

## Cross-stack update latency (no subscriptions)

- **What:** FCM data push delivery latency is ~1–3 seconds. Without GraphQL subscriptions, there's no second channel for sub-second updates.
- **Why:** subscriptions add WebSocket auth, reconnection logic, and battery cost on mobile — unjustified at v1 scale.
- **Cost / mitigation:** the v1 UX accepts 1–3 second visibility delays for shared-resource updates. A user looking at a shared timetable while a moderator edits won't see the change instantly. The schema is modelled to allow subscriptions in v2 without breaking changes.
