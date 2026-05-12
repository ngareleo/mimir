This file covers single points of failure and backup gaps in v1.

## Single Fly machine

- **What:** the Rust server runs on a single `shared-cpu-2x` machine in `jnb`. No HA, no multi-region.
- **Why:** v1 traffic is too small to justify multi-region cost or failover complexity. The server is stateless so horizontal scale is mechanical when needed.
- **Cost / mitigation:** if the JNB machine fails, the app is down until Fly restarts it (usually <60s). Fly's autorestart-on-crash policy is the only mitigation. See `docs/architecture/14-deployment/01-fly-server.md`.

## AuraDB Free auto-pauses dev/staging

- **What:** dev and staging Neo4j instances auto-pause after three days of inactivity.
- **Why:** AuraDB Free is the only zero-cost option that matches the prod Cypher contract.
- **Cost / mitigation:** the first request after a pause cold-starts the DB (~30s). Tests run from fresh service containers per workflow, so CI isn't affected. Manual wakeup is a console click or the next deploy's `migrate` step.

## No backups for AuraDB Free

- **What:** only Pro tier includes Neo4j-managed daily snapshots.
- **Why:** Free is non-production by design.
- **Cost / mitigation:** dev/staging data loss is recoverable via the seed scripts in `apps/server/tests/seed/`. Prod (Pro) is backed up daily with seven-day retention.

## No out-of-band Tigris backups

- **What:** no manual snapshot rotation or cross-region replication of Tigris buckets.
- **Why:** Tigris is globally distributed under the hood; cross-region redundancy is implicit.
- **Cost / mitigation:** we trust Tigris's durability claims. If business needs change, an `s3 sync` against a parallel bucket is straightforward to add.
