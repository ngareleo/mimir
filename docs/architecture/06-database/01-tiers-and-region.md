This file covers the AuraDB tier choices and the region tradeoff.

## Tiers

| Environment | Tier | Region | Notes |
|---|---|---|---|
| Local dev | AuraDB Free | per-developer | Each engineer provisions their own free instance. |
| Staging | AuraDB Free | shared | One Free instance, recreated freely. |
| Production | AuraDB Pro Starter | `eu-west` | ~$65/month. Backups and uptime guarantees. |

## Region tradeoff

The server runs in Fly.io `jnb` (Johannesburg) to serve Kenyan users with low last-mile latency. AuraDB's nearest region to that audience is `eu-west`. The server-to-DB hop sits at roughly 150ms round-trip.

We accept this tradeoff. The user-perceived metric is server-to-client, which is fast; the DB hop is amortised across the work the server is already doing for the request.

## Mitigations

- **Minimise round-trips per GraphQL operation.** One Cypher query per resolver wherever possible; aggregate sub-resolver loads into a single batched query when N+1 emerges.
- **Lean on the Ferry persisted cache** on the client to absorb repeat reads. See `docs/architecture/10-offline-and-sync/01-cache-strategy.md`.
- **Connection pool warm.** The server keeps a `neo4rs` pool of long-lived connections so each request does not pay the TLS handshake cost.

## What we did not pick

- **Self-hosting Neo4j on Fly.** Ruled out: we are a small team and AuraDB removes the operational burden.
- **A Neo4j region closer to `jnb`.** AuraDB does not offer one in v1 of our deployment; `eu-west` is the practical floor.
- **Moving the server out of `jnb` to reduce DB latency.** That would move the higher-latency hop onto every client request, which is worse for the user experience.
