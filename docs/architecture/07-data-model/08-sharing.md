This file covers the `:SharedOwnership` and `:ShareableLink` labels.

## `:SharedOwnership`

A `:SharedOwnership` node is the join point when a resource is shared between users with non-trivial rules (approval, moderation). It is reified — promoted from an edge to a node — because multiple users *and* the shared resource all need to point at the same state.

Properties:

- `id` — UUIDv7.
- `resourceLabel` — the label of the shared resource (e.g. `Semester`, `Timetable`).
- `policy` — `MODERATED` | `OPEN`.
- `createdAt`, `updatedAt`.

### Edges

- Incoming: `(:Semester | :Timetable)-[:LINKED_TO]->(:SharedOwnership)` — the resource the share covers.
- Incoming: `(:User)-[:MEMBER_OF]->(:SharedOwnership)` — every participating user.
- Incoming: `(:User | :Superuser)-[:MODERATES]->(:SharedOwnership)` — moderators (a subset of members).

Approval semantics — who must approve what — live in `docs/architecture/11-sharing-and-permissions/03-approval-flow.md`.

## `:ShareableLink`

A `:ShareableLink` is a single-use or multi-use opaque-token handle that anyone can follow to access a resource without an account.

Properties:

- `id` — UUIDv7.
- `token` — high-entropy random string; unique.
- `expiresAt` — optional ISO-8601 UTC.
- `revokedAt` — optional ISO-8601 UTC.
- `createdAt`.

### Edges

- Incoming: `(resource)-[:SHARED_VIA]->(:ShareableLink)` — the resource the link grants access to.

## Why reify shared ownership

A simple `[:SHARED_WITH]` edge between user and resource cannot carry approval state because the state has to be the same for every member of the share. The `:SharedOwnership` node is the single place that state lives. Members and moderators attach to it directly.
