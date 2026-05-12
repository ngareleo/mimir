This file covers the notification transport model.

## Two transports, two responsibilities

| Type set | Transport | Source of truth |
|---|---|---|
| **Types 1–5** (lectures, assignments, CATs, exams, school deadlines) | OS-scheduled local notifications via `flutter_local_notifications` | `scheduled_notifications` Hive box on the device |
| **Types 6–8** (shared-resource change, invite, approval-pending) | FCM data-only push via `firebase_messaging` | server, fanned out per mutation |

The product-side taxonomy is in `docs/product/06-notifications/index.md`.

## Why the split

- **Types 1–5 are personal and time-based.** The device knows when to fire them — the server does not need to be involved at the moment of delivery. Scheduling locally avoids paying push round-trips and works offline.
- **Types 6–8 are event-triggered by other users.** They cannot be pre-scheduled; the server has to push them when the triggering event happens.

## Why FCM data-only for types 6–8

Data-only FCM messages let the client compose the user-facing notification, decide whether to show it, and use the payload to refresh local state. A notification-style FCM payload would have rendered the message immediately without giving the app a chance to reconcile its local cache.

The bridge from an incoming data message to a refreshed Ferry cache and a rescheduled local notification is documented in `docs/architecture/09-notifications/03-push-to-cache-bridge.md`.

## Web caveat

Web has no scheduled-alarm primitive. On the web target, types 1–5 degrade to FCM push delivered by a service worker. The schedule still lives on the server (computed from upcoming due dates), but the device cannot pre-schedule local fires. See `docs/architecture/03-client/03-platforms.md`.
