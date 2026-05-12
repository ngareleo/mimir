# Sharing and permissions

Resources in Mimir can be owned by one user, shared with others through moderated membership, or made accessible via shareable links. This folder documents who can do what, how approvals work for shared resources, and how the server propagates change to the affected users. The data model side of sharing lives in `docs/architecture/07-data-model/08-sharing.md`.

## Direct children

- [00-overview.md](00-overview.md) — the permission model end-to-end.
- [01-ownership.md](01-ownership.md) — who owns what; the `:OWNS` edge.
- [02-moderators.md](02-moderators.md) — moderation rights via `:MODERATES`.
- [03-approval-flow.md](03-approval-flow.md) — how a change to a moderated resource is reviewed and applied.
- [04-fan-out.md](04-fan-out.md) — best-effort `tokio::spawn` notification fan-out.
