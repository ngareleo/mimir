This file covers the approval flow for changes to shared resources.

## When approval is required

A change to a resource governed by a `:SharedOwnership` node with `policy = MODERATED` requires approval if the proposer is *not* the owner and *not* a moderator. Owner-initiated changes apply immediately. Moderator-initiated changes still go through the approval queue unless the moderator is also the owner.

`policy = OPEN` shares apply changes immediately for every member; the data model still uses `:SharedOwnership` so future tightening to `MODERATED` requires no migration.

## The proposal

A change is submitted as a GraphQL mutation that targets the shared resource. The server:

1. Detects that the proposer's authority is insufficient for immediate apply.
2. Records the change as a `:Proposal` node attached to the `:SharedOwnership`, with the requested diff in properties.
3. Fans out an approval-pending notification (type 8) to the owner and every moderator. See `docs/architecture/11-sharing-and-permissions/04-fan-out.md`.

## Resolution

The owner or any moderator runs one of two mutations:

- `approveProposal(proposalId: ID!)` — applies the diff to the resource in the same transaction that marks the proposal as approved, then fans out a shared-change notification (type 6) to every member.
- `rejectProposal(proposalId: ID!, reason: String)` — marks the proposal as rejected; fans out a notification only to the proposer.

One approver suffices. We do not implement N-of-M approval in v1.

## What gets approved

Edits to fields on the shared resource and its composed children (a unit's lectures, a timetable's slots) require approval. Personal data linked to the shared resource (a member's own tasks `[:LINKED_TO]` it) does not — those are owned by the proposing member.

## Race handling

If two members propose conflicting changes, both are queued. The first approval wins; the second proposal is marked as superseded and the proposer notified. No automatic merging.

## Audit trail

`:Proposal` nodes are not deleted on resolution. They form an append-only audit log: who proposed what, who approved or rejected, when, why.
