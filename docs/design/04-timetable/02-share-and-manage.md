# Share and manage

This file covers the management surface for an existing timetable.

## Layout — top

- Global app bar with back arrow and title **Timetable**.
- A centered avatar above the timetable label and a secondary line with the owner's handle.

## Layout — action list

A vertical list of icon-prefixed actions:

1. **Invite someone** — opens an invite flow for adding members.
2. **Share invite** — produces a shareable invite reference via the platform share sheet.
3. **More information** — opens a metadata view (creation date, description, parent semester if any).
4. **Delete semester** — destructive action that removes the timetable; confirmation is required.

Note: the destructive label reads **Delete semester** in the mockup; in implementation it deletes the resource being managed (the timetable) and the wording should be reconciled.

## Layout — moderators and members

Below the action list, two grouped sections appear in order:

- **Moderators** — a list of users with elevated permissions, each row showing avatar, display name, email, and a trailing overflow affordance for per-row actions (promote, demote, remove).
- **Members** — a list of users with view access, with the same row layout and overflow.

The three-tab bottom navigation persists.

## Behaviour

The overflow on a moderator or member row exposes role and membership actions, governed by the rules in `docs/architecture/11-sharing-and-permissions/02-moderators.md`. **Invite someone** and **Share invite** both produce invite references; the resulting invite notification flow is described in `docs/product/06-notifications/02-event-based.md`.
