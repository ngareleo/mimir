This file covers AuraDB provisioning and configuration.

## Tiers

- **Dev / staging:** AuraDB Free. 200,000 nodes / 400,000 relationships. Auto-pauses after three days of inactivity — acceptable outside production; the migration binary wakes the instance on the next deploy.
- **Prod:** AuraDB Pro Starter. No pause, larger limits, daily backups managed by Neo4j.

## Region

Prod AuraDB lives in `gcp-europe-west1`. Neo4j has no Africa region. The server in Fly `jnb` pays ~150ms RTT per Bolt roundtrip. Accepted in v1 as the tradeoff for keeping the client↔server hop fast for Kenyan users. See [00-overview.md](00-overview.md) for the latency-budget reasoning.

## Connection

The server connects via Bolt over TLS (`neo4j+s://<host>:7687`). Credentials live as Fly secrets:

- `NEO4J_URI`
- `NEO4J_USER`
- `NEO4J_PASSWORD`

The `neo4rs` driver maintains a small connection pool sized to twice the Tokio runtime's worker-thread count.

## Schema management

Schema lives in `apps/server/migrations/*.cypher` and is applied by the `migrate` cargo binary on every deploy as Fly's `release_command`. See `docs/architecture/06-database/02-migrations.md`.

Constraints and indexes are part of the migration set. See `docs/architecture/06-database/03-constraints-and-indexes.md`.

## Backups

Pro tier includes daily snapshot backups retained for seven days. Restore is a console operation. Out-of-band backups via `neo4j-admin database dump` are not automated in v1.
