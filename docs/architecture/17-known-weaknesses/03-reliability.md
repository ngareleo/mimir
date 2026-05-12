This file covers accepted-loss patterns and missing test coverage.

## Best-effort FCM fan-out

- **What:** the server uses `tokio::spawn` (fire-and-forget) to push to FCM after a shared-resource mutation. Tasks in flight at SIGTERM are lost.
- **Why:** queues, retries, and durable outboxes are infrastructure debt v1 can't justify. Fly deploys are infrequent in normal operation.
- **Cost / mitigation:** a deploy mid-burst can drop pushes for a handful of users. Compensating mechanism: the client reconciles its `scheduled_notifications` Hive box from the cache on the next foreground, so the missing local notification eventually gets scheduled. Server state is correct after the mutation commits. See `docs/architecture/11-sharing-and-permissions/04-fan-out.md`.

## No offline writes

- **What:** all GraphQL mutations require connectivity. Offline, the client shows an "offline" state and the write fails.
- **Why:** a write queue with conflict resolution is a large engineering surface, and the most common offline case (creating personal tasks in class) is more tolerant of "wait for WiFi" than of stale-data conflicts on reconnect.
- **Cost / mitigation:** a student in a Kenyan classroom with patchy WiFi can't add a task until they return to connectivity. Documented in `docs/architecture/10-offline-and-sync/02-write-policy.md`.

## No end-to-end tests in v1

- **What:** no automated test exercises the client → server → DB path together.
- **Why:** building and maintaining an E2E harness pre-launch has poor ROI; tests catch issues that show up faster in human use.
- **Cost / mitigation:** widget + golden tests on the client and resolver tests on the server cover their respective layers. Regressions that span the boundary will surface through user feedback and Sentry. See `docs/architecture/15-testing/00-overview.md`.

## Single delivery attempt per push

- **What:** each FCM data message is sent once. There's no retry on transient FCM failures.
- **Why:** retries would need a queue; this is the same constraint as the fan-out item above.
- **Cost / mitigation:** transient FCM failures are rare in practice. Recovery is the same foreground-reconciliation path.
