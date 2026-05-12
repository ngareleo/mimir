This file covers resource ownership.

## The `:OWNS` edge

Every top-level resource has exactly one owner, recorded as `(:User)-[:OWNS]->(:Resource)`. Owners exist for:

- `:Semester`
- `:Timetable`
- `:Task`
- `:Event`

`:Unit`, `:Lecture`, `:Assignment`, and `:Paper` do not carry their own `:OWNS` edge; they inherit their owner from the `:Semester` (or `:Unit`) that contains them.

## What ownership grants

The owner can:

- Read, edit, and delete the resource without going through the approval flow.
- Add and remove members and moderators.
- Create and revoke `:ShareableLink`s.
- Transfer ownership (a single mutation that re-points the `:OWNS` edge; subject to the new owner accepting).

## Composition and ownership

When a resource is composed inside another (`(:Semester)-[:CONTAINS]->(:Unit)`), the contained resource shares the parent's ownership. Editing a unit requires the same authority as editing the containing semester. The model does not duplicate `:OWNS` edges for composed children.

## What ownership does not grant

- Ownership of a resource does not imply ownership of a `:User`'s data on that resource. A user who is a *member* of a shared semester still owns their own tasks and events that point at it via `:LINKED_TO`. The owner cannot edit a member's personal tasks.
- Ownership does not grant cross-resource moderation rights. Each resource's `:MODERATES` set is local to that resource.

## Single-owner rule

A resource cannot have two owners. Multi-user authority is expressed by promoting members to moderators on a `:SharedOwnership` node — see `docs/architecture/11-sharing-and-permissions/02-moderators.md`. This keeps the ownership graph acyclic and easy to query.
