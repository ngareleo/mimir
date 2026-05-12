This file covers what happens when the device returns online after an offline period.

## Reads

When connectivity is restored, no special bulk-resync runs. The next time a query is opened, Ferry's cache-first policy serves the cached value and immediately refetches in the background. Updated data flows in through normalisation.

This is intentional: we do not pay a "reconnect storm" of refetches for queries the user is not currently looking at. Stale data in the cache stays stale until either it is read or its staleness threshold evicts it.

## Foreground reconciliation

When the app comes to the foreground (which often coincides with reconnecting), three reconciliations run:

1. **Auth refresh.** If the access token is near expiry, the auth link rotates it before user-initiated requests run.
2. **Notification reconciliation.** The `scheduled_notifications` Hive box is rebuilt against the current cache and the iOS rolling window is re-issued. See `docs/architecture/09-notifications/01-local-scheduling.md`.
3. **Sentry buffer flush.** Any breadcrumbs and crash reports queued offline are sent.

## Writes

There is nothing to reconcile on the write side, because nothing was queued. Any mutations the user wanted to run while offline were never issued and never reached the server. The user re-attempts them when they choose to.

## Shared-resource changes that happened while offline

If a moderator approved a change to a resource the user has cached while the user was offline:

1. The FCM data message that announced the change was missed (FCM does not deliver to offline devices indefinitely — it has its own retention).
2. On reconnect, the cache still reflects the pre-change state.
3. The first time the user opens that resource, the cache-first policy serves the stale view briefly and the background refetch corrects it.

For the schedule, the post-foreground reconciliation step rebuilds `scheduled_notifications` from the refreshed cache, so missed shared-change notifications do not cause stale alarms.

This best-effort behaviour matches the server's best-effort fan-out (see `docs/architecture/11-sharing-and-permissions/04-fan-out.md`); both ends are tolerant of message loss.
