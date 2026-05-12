This file covers how the server fans out push notifications for shared-resource events.

## Best-effort `tokio::spawn`

When a mutation produces an event that should be pushed (shared change, invite, approval-pending), the resolver records the database state in the same transaction as the mutation, then spawns a Tokio task to deliver FCM data messages to the affected users' tokens. The spawn is fire-and-forget: the resolver does not await it before responding to the client.

## Why best-effort

- Awaiting fan-out would block GraphQL responses on third-party (FCM) availability.
- Retries inside the request would amplify FCM rate-limit problems.
- Bouncing fan-out into a queue with its own delivery guarantees is more infrastructure than v1 needs.

The cost: pushes can be lost when a Fly machine receives SIGTERM mid-fan-out (during a deploy or a scaling event). The product accepts this loss in v1 because of two compensating mechanisms:

1. **Server state is already correct.** The mutation committed before the spawn. The next time the affected client refreshes, the new state arrives through the normal cache path.
2. **Foreground reconciliation** on the client rebuilds the `scheduled_notifications` box from the cache. A missed FCM message that would have rescheduled a local notification is caught up the next time the app foregrounds. See `docs/architecture/09-notifications/03-push-to-cache-bridge.md`.

## Failure logging

Every spawn task is wrapped to capture errors and emit an OTel error event with the affected user IDs, the FCM error code (token invalid, rate-limited, transport error), and the originating mutation's trace ID. Failures are visible in Axiom dashboards.

Tokens FCM reports as `UNREGISTERED` or `INVALID_ARGUMENT` are deleted from the corresponding `:FcmToken` nodes inside the spawn task.

## What this is not

- **Not a queue.** There is no Redis, no SQS, no dedicated worker. The spawn lives in the request-handling process.
- **Not retried.** A single delivery attempt per recipient per event. Subsequent foreground reconciliation on the client is the recovery path.
- **Not ordered.** Two events emitted in quick succession may deliver in either order; clients are expected to render whatever cache reconciliation produces.
