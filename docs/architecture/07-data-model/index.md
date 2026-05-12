# Data model

The data model re-expresses the entities from the original Kenyatta University paper (figures 6 and 17) as a Neo4j graph. Node labels correspond to the canonical entities; relationships replace the relational join tables. The product-side description of the same entities lives in `docs/product/05-domain-model/index.md`.

## Node labels

`:User`, `:Semester`, `:Unit`, `:Lecture`, `:Timetable`, `:Task`, `:Event`, `:Assignment`, `:Paper`, `:Superuser`, `:SharedOwnership`, `:ShareableLink`.

## Relationship labels

`[:OWNS]`, `[:MODERATES]`, `[:CONTAINS]`, `[:SCHEDULED_AT]`, `[:LINKED_TO]`, `[:MEMBER_OF]`, `[:SHARED_VIA]`, `[:ENROLLED_IN]`.

Conventions for both are in [01-naming-and-ids.md](01-naming-and-ids.md).

## Direct children

- [00-overview.md](00-overview.md) — the model as a whole; what is a node, what is an edge, what is a property.
- [01-naming-and-ids.md](01-naming-and-ids.md) — labels, relationships, ID strategy.
- [02-user.md](02-user.md) — `:User` and `:Superuser`.
- [03-semester.md](03-semester.md) — `:Semester` and its ownership.
- [04-unit-lecture.md](04-unit-lecture.md) — `:Unit` and `:Lecture`.
- [05-timetable.md](05-timetable.md) — `:Timetable` and how it schedules lectures.
- [06-task-event.md](06-task-event.md) — `:Task` and `:Event`.
- [07-assignment-paper.md](07-assignment-paper.md) — `:Assignment` and `:Paper`.
- [08-sharing.md](08-sharing.md) — `:SharedOwnership` and `:ShareableLink`.
