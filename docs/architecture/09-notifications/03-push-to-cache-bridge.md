This file covers how an FCM data message becomes a refreshed cache and a rescheduled set of local notifications.

## The flow

```
FCM data message
  → Ferry cache invalidation
  → refetch of affected queries
  → reconcile scheduled_notifications Hive box
  → re-issue iOS rolling window of OS-scheduled local notifications
```

## Step by step

1. **FCM data message arrives.** The `firebase_messaging` handler dispatches to a Riverpod provider that owns the bridge.
2. **Map `resourceLabel` + `resourceId` to affected Ferry queries.** Mimir maintains a small mapping table from (label, mutation kind) to the queries whose results that change can invalidate. The provider invalidates each.
3. **Ferry refetches.** Each invalidated query is re-issued. Ferry writes the new results into its normalised cache.
4. **Reconcile `scheduled_notifications`.** A reconciler reads the refreshed queries (tasks with `dueAt`, events with `startsAt`, lectures with `:SCHEDULED_AT` slots) and recomputes the expected schedule. It diffs against the current Hive box, then applies adds/updates/deletes.
5. **Re-issue the iOS rolling window.** With the box up to date, the next-64-fires set is re-scheduled with the OS. See `docs/architecture/09-notifications/01-local-scheduling.md`.
6. **Optionally render the inbound notification.** For invites and approval-pendings, the bridge composes a local notification immediately so the user sees it. The data payload's `kind` decides whether this step runs.

## Why this indirection

- The FCM message does not carry enough state to update views; it is a signal, not a snapshot.
- The cache is the read source of truth; the schedule is derived from it. Rebuilding the schedule from the cache rather than from the FCM message keeps the schedule consistent even if multiple messages arrive in quick succession.
- Failures are visible: if a refetch fails, the box stays at its last consistent state. The next foreground reconciliation catches up.

## Failure modes

- **Refetch fails (offline, server error).** The box does not change; the user retains the previous schedule and Sentry captures the failure. Next foreground retries.
- **OS reschedule fails.** Logged as a Sentry breadcrumb; the box still holds the source of truth and a later trigger will retry.
