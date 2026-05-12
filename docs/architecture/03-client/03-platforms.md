This file covers the three client targets and where they diverge.

## Targets

- **Android** — primary mobile target for the Kenyan student audience.
- **iOS** — secondary mobile target; Sign in with Apple deferred (see `docs/architecture/08-authentication/04-apple-deferred.md`).
- **Web** — full read/write parity with mobile, with one documented gap around notifications.

## Where targets diverge

### Notifications

- **Mobile (Android + iOS)**: notification types 1–5 are scheduled locally via `flutter_local_notifications`. Types 6–8 arrive as FCM data-only messages and are turned into local notifications by the app.
- **Web**: there is no scheduled-alarm primitive in the browser. Notification types 1–5 degrade to FCM push delivered by a service worker. The product accepts this gap; see `docs/product/06-notifications/index.md` and `docs/architecture/09-notifications/02-fcm-push.md`.

### iOS pending-cap

iOS enforces a 64-pending local notification cap. The client maintains a rolling window over `scheduled_notifications`, reissuing the next window's notifications on app foreground and after every inbound FCM data message. Detail in `docs/architecture/09-notifications/01-local-scheduling.md`.

### Auth callbacks

- **Mobile**: Google sign-in returns through a custom URL scheme handled by the platform.
- **Web**: Google sign-in returns through `/auth/google/callback` on the server, which then redirects the browser back to the app with the issued tokens. See `docs/architecture/08-authentication/01-google-oauth.md`.

### Storage

Hive's web implementation uses IndexedDB. The Ferry persisted cache and the `scheduled_notifications` box both work on web without code changes.

## What stays the same

GraphQL operations, mutation semantics, the offline-write rule, Sentry instrumentation, and the route table.
