This file covers how sharing, admin, and moderator status are recorded in the graph.

The prototype's relational ERD modelled this as two join tables — `SharedOwnership` and `Superusers` (Fig 6 p.28) — needed because the relational model cannot store roles on an edge. The rebuild folds both into a single relationship between User and shareable resource.

**The share relationship.**

```
(:User)-[:SHARED_WITH {role, appointedBy, dateAdded}]->(:Semester | :Timetable)
```

Properties on the edge:

- `role` — one of `ADMIN`, `MODERATOR`, `MEMBER`. The owner has role `ADMIN`. Newly-invited users start as `MEMBER`. The admin promotes a member to moderator by changing this property.
- `appointedBy` — the user id of the admin who appointed a moderator. Null for admins and members.
- `dateAdded` — when the user joined the resource (or, for the admin, when the resource was first shared).

Only Semester and Timetable accept the `:SHARED_WITH` relationship in v1. Other entities (Unit, Lecture, Task, Event, Assignment, Paper) inherit visibility through their containing resource.

**The ownership relationship.**

```
(:User)-[:OWNS]->(:Resource)
```

`OWNS` is independent of `SHARED_WITH`. A user always has `OWNS` to their resources; on sharing, the resource additionally gains a `SHARED_WITH {role: ADMIN}` edge from the same user. The redundancy makes "list all my owned resources" and "list all resources I have access to" cheap to query in either direction.

**Why this matters for queries.** A single Cypher traversal from a User can answer: what do I own, what am I shared on, what do I moderate, who else is on this resource, and who has approval rights. None of these require a join.

For the storage-side detail (constraints, indexes, label conventions), see `docs/architecture/07-data-model/08-sharing.md`.
