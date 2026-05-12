# Offline and sync

Mimir reads work offline by serving from the Ferry persisted cache. Writes require connectivity — there is no offline mutation queue. When the device comes back online, reads reconcile naturally through the cache; writes happen at the moment the user takes the action, with a clear UI state if they cannot. The strict contract is what makes the model simple.

## Direct children

- [00-overview.md](00-overview.md) — the offline contract.
- [01-cache-strategy.md](01-cache-strategy.md) — Ferry persisted Hive cache, eviction policy.
- [02-write-policy.md](02-write-policy.md) — writes require connectivity; the offline UX rule.
- [03-reconciliation.md](03-reconciliation.md) — what happens when the device returns online.
