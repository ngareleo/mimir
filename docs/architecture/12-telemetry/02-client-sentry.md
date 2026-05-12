This file covers Sentry on the client.

## SDK

The Flutter Sentry SDK (`sentry_flutter`) is initialised at app startup, before the first widget mounts, so the framework error handler can be installed in time to catch early failures.

## What gets captured

- **Uncaught Dart exceptions** propagated to the zone error handler.
- **Flutter framework errors** via `FlutterError.onError`.
- **Manual `Sentry.captureException`** calls inside catch blocks where context matters (failed mutations, FCM handler errors, notification scheduling errors).

Sentry is *not* used to capture expected user errors (validation failures, offline-write blocks, auth failures the UI already surfaces). Those are not incidents.

## Breadcrumbs

Breadcrumbs are short structured events that accompany a crash. The client emits them at:

- **Route changes** — `go_router` redirects feed Sentry the from/to names.
- **GraphQL operations** — every issued operation logs a breadcrumb with name, kind, and outcome (success/error code). Variables are never logged.
- **Notification scheduling** — adds, deletes, and rolling-window reissues.
- **Sign-in/sign-out** — the auth provider's transitions.

## Releases and versions

Each build is tagged with a release identifier matching the app version + build number. Sentry deduplicates by release so a fix can be matched to a regression.

## Privacy and PII

Sentry is configured with `sendDefaultPii = false`. We attach:

- `user.id` — the Mimir user ID.
- `device` and `os` defaults.

We do *not* attach email, name, request bodies, or response bodies.

## Web

On web, the Sentry SDK uses the JavaScript bridge. Source maps for the web build are uploaded as part of the CI/CD step that publishes the web bundle. See `docs/architecture/14-deployment/03-ci-cd.md`.

## Trace tagging

Every Sentry event carries a `traceId` tag matching the W3C trace ID in flight at the time. Detail in `docs/architecture/12-telemetry/03-trace-correlation.md`.
