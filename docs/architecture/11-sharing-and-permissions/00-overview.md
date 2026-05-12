This file covers the permission model at a glance.

## Three permission levels

1. **Owner.** The user holds full authority over the resource and everything composed beneath it. Recorded as a `(:User)-[:OWNS]->(:Resource)` edge.
2. **Moderator.** A user (or `:Superuser`) with approval rights over changes to a shared resource. Recorded as `(:User|:Superuser)-[:MODERATES]->(:SharedOwnership)` or directly on the resource for non-shared moderation cases.
3. **Member.** A non-owner participant in a shared resource. Recorded as `(:User)-[:MEMBER_OF]->(:SharedOwnership)` or `(:User)-[:ENROLLED_IN]->(:Semester)`.

Public access through a `:ShareableLink` is a fourth path that bypasses the user identity entirely; it grants read access only.

## What each level can do

| Action | Owner | Moderator | Member | Link visitor |
|---|---|---|---|---|
| Read the resource | yes | yes | yes | yes |
| Edit (no approval) | yes | — | — | — |
| Propose an edit | — | yes | yes | — |
| Approve a proposal | yes | yes | — | — |
| Invite new members | yes | — | — | — |
| Promote a member to moderator | yes | — | — | — |
| Revoke access | yes | — | — | — |

A non-shared resource has only an owner; the table collapses to "owner can do everything, no one else can do anything".

## Where it lives

- Ownership and moderation are properties of the *graph* — edges between users and resources. No separate ACL table.
- Approval state lives on the `:SharedOwnership` node and on the proposed-change records (covered in `docs/architecture/11-sharing-and-permissions/03-approval-flow.md`).
- Fan-out of approval-required and approved-change notifications goes through the best-effort path documented in `docs/architecture/11-sharing-and-permissions/04-fan-out.md`.
