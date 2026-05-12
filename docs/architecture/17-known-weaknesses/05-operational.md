This file covers operational debt: tooling, process, infrastructure.

## Custom Cypher migration runner

- **What:** the `migrate` cargo binary is written in-house rather than using `neo4j-migrations` (JVM).
- **Why:** avoiding a Java toolchain in CI and the prod image. Migration logic is simple — read directory, hash, apply in order, record.
- **Cost / mitigation:** edge-case bugs we'd inherit for free from a battle-tested tool are now our problem. Mitigated by tests against the dev Neo4j and the small surface area of what the runner does. See `docs/architecture/06-database/02-migrations.md`.

## SDL drift caught only by CI

- **What:** the pre-commit hook regenerates `packages/graphql-schema/schema.graphql`; if a developer commits with `--no-verify`, the hook is skipped. CI's drift check is the safety net.
- **Why:** local hooks cannot be made mandatory across all developer machines.
- **Cost / mitigation:** a drift-introducing PR fails CI on the SDL check rather than at runtime. Acceptable feedback latency. See `docs/architecture/04-network-and-graphql/02-schema-source-of-truth.md`.

## No rate limiting in v1

- **What:** the GraphQL endpoint has no per-IP or per-token rate limits.
- **Why:** v1 traffic is small; abuse paths haven't materialised.
- **Cost / mitigation:** Fly's platform-level abuse protections cover gross attacks. Per-resolver limits will be added if a real pattern emerges.

## Single Tokio runtime, no workload separation

- **What:** the same Tokio runtime handles GraphQL requests, FCM fan-out tasks, and the Tigris signing calls.
- **Why:** simplicity. A separate runtime for background work is over-engineering at v1 scale.
- **Cost / mitigation:** a burst of GraphQL traffic can starve background tasks. Watched via OTel; mitigation would be a runtime split if observed.

## Migration rollbacks are manual

- **What:** the `migrate` runner applies forward only. There is no `down` step.
- **Why:** rollback semantics in Cypher are inconsistent and writing reliable down-migrations costs more than restoring from a backup.
- **Cost / mitigation:** rollback path is "restore from yesterday's AuraDB Pro snapshot". Documented as the formal recovery procedure.
