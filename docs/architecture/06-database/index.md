# Database

The database is Neo4j AuraDB. Mimir's domain is dominated by relationships — a user owns semesters, a semester contains units, a unit has lectures, a timetable schedules lectures, tasks belong to events, sharing crosses every entity — so the graph model is treated as first-class. The original Kenyatta University prototype used Postgres + MongoDB and ran into cross-entity complexity that a graph dissolves. Tiers, migrations, and constraints are documented here.

## Direct children

- [00-overview.md](00-overview.md) — why Neo4j, why a graph, what we are not optimising for.
- [01-tiers-and-region.md](01-tiers-and-region.md) — AuraDB Free for dev/staging, Pro Starter `eu-west` for prod, latency tradeoff.
- [02-migrations.md](02-migrations.md) — `.cypher` files, the `migrate` binary, Fly `release_command`.
- [03-constraints-and-indexes.md](03-constraints-and-indexes.md) — uniqueness constraints per label, index policy.
