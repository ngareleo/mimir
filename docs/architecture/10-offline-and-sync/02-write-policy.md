This file covers the offline-write policy and the UX contract that goes with it.

## Policy

Mutations are not queued. If the device cannot reach the server, the mutation does not run. The user is told, clearly, that they are offline and the action will be available when they reconnect.

## UI contract

- **Action buttons disable** when the connectivity provider reports offline. A short label explains why ("offline").
- **Forms permit composition offline** — drafting a new task is fine — but submission is gated.
- **Optimistic UI is not used** for any mutation in v1. The cache reflects the server's truth, never an unresolved local intention.
- **A global "you are offline" banner is not used.** The state is conveyed per-action, where it is actionable.

## Why no queue

- A queue would need ordering: mutation A then mutation B then mutation C, replayed in order, with the right user context.
- A queue would need conflict handling: what if the resource was modified by another device or another user in the interim?
- A queue would need failure UX: each queued mutation can fail differently when it finally runs, and we would need a way to surface that to the user.

Each of those is a small project. None is solved well by "best effort" defaults. We defer the problem until v2 by making writes synchronous from the user's point of view.

## Connectivity detection

A single Riverpod provider exposes connectivity status, backed by `connectivity_plus`. The provider is the *only* signal that gates writes; it is reused by:

- Mutation buttons (for the disable state).
- The Ferry auth link (to short-circuit retries that cannot succeed).
- The notification scheduler (to skip server-touching reconciliation when offline).

There is no per-mutation connectivity check. The provider is the source of truth.

## Reads vs writes

Reads continue working offline through the cache. The contract is asymmetric on purpose: read-side staleness is acceptable, write-side staleness (queued writes) is not.
