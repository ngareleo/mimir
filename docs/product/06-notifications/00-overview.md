This file covers the eight notification types Mimir can deliver.

Notifications fall into two categories with different reliability guarantees. **Time-based** notifications (types 1–5) are personal reminders tied to a known future time; they are scheduled on the device and fire without connectivity. **Event-based** notifications (types 6–8) communicate state changes initiated by other users; they require connectivity and arrive via FCM.

| # | Type | Category | Triggered by |
|---|---|---|---|
| 1 | Lecture upcoming | Time-based | A lecture's `timeFrom` approaching. |
| 2 | Assignment deadline | Time-based | An assignment's `dueDate` approaching. |
| 3 | CAT / assessment test | Time-based | A `Paper` with kind `CAT` or `ASSESSMENT` approaching. |
| 4 | Exam / paper | Time-based | A `Paper` with kind `EXAM` approaching. |
| 5 | School deadline | Time-based | A user-defined deadline (fee payment, unit registration). |
| 6 | Shared-resource change | Event-based | An admin, moderator, or approved member changes a shared resource the user is on. |
| 7 | Resource shared with you | Event-based | Another user shares a semester or timetable with this user. |
| 8 | Approval-pending | Event-based | A non-owner submits a change request on a resource the user moderates. |

The category is what determines transport and offline behaviour:

- Types 1–5: scheduled locally from cached domain data (see `docs/product/06-notifications/01-time-based.md`).
- Types 6–8: delivered as FCM data messages from the server (see `docs/product/06-notifications/02-event-based.md`).

For transport-side detail and the iOS rolling-window scheduler, see `docs/architecture/09-notifications/index.md`.
