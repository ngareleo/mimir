This file covers Neo4j constraints and indexes.

## Uniqueness constraints

Every node label that carries an external identifier gets a unique constraint on its `id` property. The set of labels is defined in `docs/architecture/07-data-model/index.md`.

```
CREATE CONSTRAINT user_id_unique IF NOT EXISTS
  FOR (n:User) REQUIRE n.id IS UNIQUE;
```

The same pattern applies to `:Semester`, `:Unit`, `:Lecture`, `:Timetable`, `:Task`, `:Event`, `:Assignment`, `:Paper`, `:Superuser`, `:SharedOwnership`, and `:ShareableLink`.

Additional unique constraints:

- `:User.email` — unique, case-folded at write time.
- `:ShareableLink.token` — unique; high-entropy random.

## Property existence

For nodes that must always have an identifier and a created-at timestamp, we add existence constraints:

```
CREATE CONSTRAINT user_id_exists IF NOT EXISTS
  FOR (n:User) REQUIRE n.id IS NOT NULL;
```

Existence constraints are added for `id` and `createdAt` on every entity label.

## Indexes

We add indexes only where a query path demands them:

- `:User(email)` — login lookup.
- `:Semester(ownerId)` — listing a user's semesters.
- `:Task(dueAt)` — upcoming-task queries.
- `:Event(startsAt)` — upcoming-event queries.

Composite indexes are added on a case-by-case basis as the query set stabilises. We prefer to under-index and add indexes when query latency demonstrates a need.

## Where constraints and indexes are defined

In migration files (`apps/server/migrations/*.cypher`). They are not implicit in the application; the schema is reproducible by replaying the migrations against an empty database. See `docs/architecture/06-database/02-migrations.md`.
