This file covers why Mimir uses Neo4j and why the data is modelled as a graph.

## Why a graph

The prior Kenyatta University prototype (paper §3.4) ran on Postgres for relational entities and MongoDB for unstructured documents. The split was forced by entity shape, not by access patterns. Even with that split, the relational core carried significant join-table complexity:

- A `Semester` is *owned by* a `User` but also *moderated by* zero or more other `User`s.
- A `Unit` *belongs to* a `Semester` and is *shared with* a set of `User`s through a separate ownership join.
- A `Lecture` *belongs to* a `Unit` and is *scheduled at* a slot inside a `Timetable`.
- A `Task` or `Event` can be *linked to* a `Lecture`, an `Assignment`, a `Paper`, or a `Unit` — polymorphic ownership in relational terms.
- A `SharedOwnership` cuts across `Semester`, `Unit`, `Lecture`, and `Timetable` with different approval rules per resource type.

Expressing these relationships in a relational schema means a thicket of join tables and `CHECK` constraints. The relationships *are the model*. In a graph database they become first-class edges with their own labels; queries follow paths rather than join.

## Why Neo4j specifically

- AuraDB is a managed, low-ops product. The team is small.
- The `neo4rs` driver is well-supported in Rust and matches the server's async runtime.
- The Cypher syntax is the most readable graph query language at this scale.
- Constraints, indexes, and migrations are first-class — no third-party schema tooling needed.

## What we are not optimising for

- Heavyweight analytics. Mimir is OLTP; aggregations are limited and time-bounded.
- Multi-region active-active. The DB lives in `eu-west` and the server in Fly `jnb`; we accept the hop. See `docs/architecture/06-database/01-tiers-and-region.md`.
- Document storage. Anything that would have lived in MongoDB lives in Tigris (files) or as node properties (small JSON-shaped values).
