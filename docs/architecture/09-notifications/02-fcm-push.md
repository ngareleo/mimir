This file covers FCM push for the event-based notification types.

## Library

`firebase_messaging` on Android, iOS, and web. Android uses FCM natively; iOS uses FCM as the front-end for APNs; web uses FCM with a service worker.

## Data-only messages

Server-emitted FCM messages are **data-only** — no `notification` block. The payload carries:

```
{
  kind,           // "INVITE" | "APPROVAL_PENDING" | "SHARED_CHANGE"
  resourceId,     // the resource the message is about
  resourceLabel,  // "Semester" | "Timetable" | …
  traceId         // W3C trace id of the originating mutation
}
```

The client receives the payload and decides what to do — render a local notification, refresh a query, or both. A notification-style payload (which would have rendered automatically) would short-circuit the cache-refresh step.

## Token registration

When the client signs in or rotates an FCM token, it calls a GraphQL mutation:

```
mutation { registerFcmToken(token: String!, platform: Platform!) { success } }
```

The server stores tokens on a `:FcmToken` node linked to the `:User`. The fan-out path (see `docs/architecture/11-sharing-and-permissions/04-fan-out.md`) reads tokens off these nodes when emitting pushes.

Tokens are removed on sign-out and when FCM reports them as invalid during a send.

## Server send path

Server-side, the `notify` module wraps the FCM HTTP v1 API. Sends are best-effort `tokio::spawn` tasks per recipient token. Failures are logged through OTel; pushes lost during a deploy or SIGTERM are accepted in v1. Detail in `docs/architecture/11-sharing-and-permissions/04-fan-out.md`.

## Web service worker

On web, a registered service worker receives FCM data messages and forwards them to the page, or — if the page is closed — shows a notification of its own composition. The same `kind`/`resourceId` payload drives both paths.
