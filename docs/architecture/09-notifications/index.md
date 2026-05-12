# Notifications

Notifications split into two transport models. The five personal time-based types are scheduled on the device by `flutter_local_notifications`. The three cross-user event-based types arrive as FCM data-only messages and are turned into local notifications by the app. The product-side taxonomy lives in `docs/product/06-notifications/index.md`.

## Direct children

- [00-overview.md](00-overview.md) — the transport model: local vs FCM data.
- [01-local-scheduling.md](01-local-scheduling.md) — the `scheduled_notifications` Hive box and the iOS rolling window.
- [02-fcm-push.md](02-fcm-push.md) — `firebase_messaging`, data-only messages, token registration.
- [03-push-to-cache-bridge.md](03-push-to-cache-bridge.md) — how an FCM message ends up rescheduling local notifications.
