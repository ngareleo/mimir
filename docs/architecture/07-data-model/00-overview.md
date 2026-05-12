This file covers the data model as a whole — what gets a label, what gets an edge, what gets a property.

## Shape of the graph

Every domain entity is a node with one primary label. Relationships between entities are edges with a directional label. Small scalar attributes are properties on nodes; small structured attributes (e.g. a `dayOfWeek` + `startTime` + `endTime` tuple) are properties on the *edge* that carries the scheduling semantics rather than a side node.

Example slice (in Cypher):

```
(:User)-[:OWNS]->(:Semester)-[:CONTAINS]->(:Unit)-[:CONTAINS]->(:Lecture)
(:Timetable)-[:SCHEDULED_AT {dayOfWeek, startTime, endTime}]->(:Lecture)
```

## What is a node

Anything an end-user can reference directly — that has its own identity, lifecycle, ownership, and that other entities can point at — is a node. The full list is in `docs/architecture/07-data-model/index.md`.

## What is an edge

Edges express *how* nodes relate. Edge labels are tense-neutral verbs (`OWNS`, `CONTAINS`, `MODERATES`). Where the relationship carries scheduling or approval state, the state lives on the edge as properties. Edges are not promoted to nodes unless multiple other nodes need to reference the relationship itself — currently only `:SharedOwnership` works that way (see `docs/architecture/07-data-model/08-sharing.md`).

## What is a property

Small values whose lifetime is tied to the node: scalars (`id`, `createdAt`, `email`, `name`), enum-shaped strings (`role`, `status`), and structured short objects encoded as JSON strings where they have no relational shape of their own.

## Properties every node carries

- `id` — opaque global ID. Strategy in `docs/architecture/16-graphql-conventions/04-ids.md`.
- `createdAt` — ISO-8601 UTC.
- `updatedAt` — ISO-8601 UTC; updated on every write.

Existence and uniqueness constraints for `id` are in `docs/architecture/06-database/03-constraints-and-indexes.md`.
