# Domain model

This folder covers the entities Mimir stores and the relationships between them. The model is expressed as a **graph** — every entity is a node, and every association is a named relationship — to reflect Mimir's actual storage in Neo4j. The original prototype used a relational ERD (paper Fig 6, p.28); see `docs/architecture/06-database/00-overview.md` for why the rebuild moved to a graph.

Property names below are descriptive; the authoritative set lives with the GraphQL schema in the server crate. For the storage-side expression of these entities (labels, indexes, ID strategy), see `docs/architecture/07-data-model/index.md`.

## Contents

- `docs/product/05-domain-model/00-overview.md` — modelling principles, glossary, and naming.
- `docs/product/05-domain-model/01-user.md` — the User node.
- `docs/product/05-domain-model/02-semester.md` — the Semester container.
- `docs/product/05-domain-model/03-unit.md` — a unit of study (course).
- `docs/product/05-domain-model/04-lecture.md` — a scheduled class for a unit.
- `docs/product/05-domain-model/05-timetable.md` — a weekly schedule of lectures.
- `docs/product/05-domain-model/06-task.md` — a one-off reminder.
- `docs/product/05-domain-model/07-event.md` — a scheduled occurrence.
- `docs/product/05-domain-model/08-assignment.md` — a typed academic deliverable.
- `docs/product/05-domain-model/09-paper.md` — exam, CAT, and assessment items.
- `docs/product/05-domain-model/10-shared-ownership.md` — how sharing, admins, and moderators are recorded.
