This file covers the `:Semester` label.

## Shape

A semester is a time-bounded container the user creates to organise a term's units. Properties:

- `id` — UUIDv7.
- `name` — human label (e.g. "Y3S1 2026").
- `startsOn`, `endsOn` — calendar dates, inclusive.
- `createdAt`, `updatedAt`.

## Edges

- Incoming: `(:User)-[:OWNS]->(:Semester)` — the owner.
- Incoming: `(:User)-[:ENROLLED_IN]->(:Semester)` — non-owner participants.
- Incoming: `(:User|:Superuser)-[:MODERATES]->(:Semester)` — moderators with approval rights.
- Outgoing: `(:Semester)-[:CONTAINS]->(:Unit)` — composes the units that make up the semester.
- Optional: `(:Semester)-[:SHARED_VIA]->(:ShareableLink)` — when a public link exists.
- Optional: `(:Semester)-[:LINKED_TO]->(:SharedOwnership)` — when more than one user has authority.

## Lifecycle

A semester is created by one user. The owner can invite participants (creating `:ENROLLED_IN` edges) and promote a participant to moderator (creating `:MODERATES`). Changes to shared content inside a semester (units, lectures, timetables) flow through the approval rules in `docs/architecture/11-sharing-and-permissions/03-approval-flow.md`.

## Why a node, not an edge

A semester has its own identity, its own creation/update lifecycle, and other entities point at it. It is unambiguously a node.

## Why composition rather than membership

The relationship from a semester to its units is structural: a unit only makes sense inside a semester. `[:CONTAINS]` captures the parent/child shape. Sharing is layered on top through `:SharedOwnership`, not by promoting `[:CONTAINS]` to a multi-owner edge.
