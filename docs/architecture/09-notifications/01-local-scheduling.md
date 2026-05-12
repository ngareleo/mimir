This file covers local notification scheduling on the device.

## Library

`flutter_local_notifications` on iOS and Android. Each target maps to OS-scheduled alarms (UNNotificationCenter, AlarmManager).

## The `scheduled_notifications` Hive box

The schedule's source of truth is a dedicated Hive box named `scheduled_notifications` — **not** the Ferry persisted cache. This box stores one record per intended fire:

```
{
  id,             // stable across reschedules
  type,           // one of the 1-5 product types
  title,
  body,
  fireAt,         // UTC ISO-8601
  payload         // small JSON, used on tap
}
```

The box is separate from the Ferry cache for two reasons:

- It survives Ferry cache eviction — losing your notification schedule because the cache reaped a query is a poor user experience.
- It is a write-side artefact: the app computes it from query results, then schedules it. Conflating it with read-cache lifecycle would make eviction reasoning fragile.

## iOS 64-pending rolling window

iOS limits an app to 64 pending scheduled notifications. Mimir handles that by:

- Sorting `scheduled_notifications` by `fireAt`.
- Issuing OS-level schedules for only the next 64 entries.
- Re-issuing the window whenever the set changes, on app foreground, and on every inbound FCM data message.

Android does not have the cap; we still apply the rolling-window logic uniformly to keep one code path.

## Reconciliation triggers

The window is recomputed and re-issued when any of these happen:

- Ferry writes a query result that changes the affected entities (a new task, an updated lecture, a cancelled event).
- An FCM data message arrives — see `docs/architecture/09-notifications/03-push-to-cache-bridge.md`.
- The app comes to the foreground.
- The user signs in (initial schedule) or signs out (clear the box).

The reconciliation step is idempotent: re-running it with no changes is a no-op.
