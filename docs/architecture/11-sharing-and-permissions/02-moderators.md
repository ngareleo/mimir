This file covers moderators on shared resources.

## Who can be a moderator

- A regular `:User` granted moderation by the resource's owner.
- A `:Superuser` (who is also a `:User`), for content under global moderation. Currently this applies only to school-deadline-style content the team curates.

A moderator is *always* a member of the shared resource — they have read access by virtue of being in the share.

## Granting moderation

The owner of a shared resource issues a GraphQL mutation `grantModeration(sharedOwnershipId: ID!, userId: ID!)`. The mutation creates a `(:User)-[:MODERATES]->(:SharedOwnership)` edge. The promotion is immediate; there is no acceptance step (the user has already accepted membership).

## Revoking moderation

The owner issues `revokeModeration(sharedOwnershipId: ID!, userId: ID!)` to remove the edge. The user remains a member.

## What moderators can do

- Approve or reject proposed changes from other members. See `docs/architecture/11-sharing-and-permissions/03-approval-flow.md`.
- Propose changes themselves; their proposals still go through the approval queue unless they are also the owner.

## What moderators cannot do

- Add or remove other members.
- Promote or demote other moderators.
- Transfer or delete the resource.
- Bypass the approval flow.

Moderation is review authority, not administrative authority. The single-owner rule (see `docs/architecture/11-sharing-and-permissions/01-ownership.md`) is preserved.

## Storage

Moderation is an edge, not a property. There is no `role` column to keep in sync — the presence or absence of the `:MODERATES` edge is the source of truth. Queries asking "who can approve this" follow incoming `[:MODERATES]` edges to the resource's `:SharedOwnership` node and union with the resource's owner.
