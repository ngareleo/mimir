This file covers types 6–8: notifications that communicate cross-user state changes and arrive via push.

These notifications cannot be scheduled locally because their trigger is another user's action. They require connectivity at delivery time. They arrive as FCM data messages from the server; the client uses them to update the cache and refresh time-based schedules.

**Type 6 — Shared-resource change.** Fires for every member of a shared semester or timetable when an authoritative change is applied — that is, when the owner edits directly, a moderator edits, or a member's change request is approved. The message identifies the resource and the changed item. Examples: a moderator adds an assignment to a shared semester, a co-rep edits a lecture's venue on a shared timetable.

**Type 7 — Resource shared with you.** Fires for the recipient when another user shares a semester or timetable with their account. The message identifies the resource and the inviter. It acts as the user's entry point into the shared-resource flow.

**Type 8 — Approval-pending.** Fires for the admin and every moderator of a shared resource when a non-owner member submits a change request. The message identifies the resource, the requester, and the item being changed. Each approver receives one notification per request. Once any approver acts on the request, the others see no further notification for it.

Event-based notifications are also the trigger for refreshing the device's time-based schedules: an FCM data message may indicate that a new assignment was added to a shared semester, which the client must turn into a new type-2 reminder. The push-to-cache-bridge mechanics live in `docs/architecture/09-notifications/03-push-to-cache-bridge.md`. The server's best-effort fan-out behaviour lives in `docs/architecture/11-sharing-and-permissions/04-fan-out.md`.
