# Notifications

This folder covers Mimir's notification taxonomy from the product's point of view: the eight types of message a user can receive, what triggers each, and the split between time-based (fires locally) and event-based (delivered via push) categories. The transport mechanism — how time-based and event-based notifications are actually scheduled and delivered — lives in `docs/architecture/09-notifications/index.md`.

The two categories exist because they have different reliability and offline behaviour. Time-based notifications must work without connectivity (a student's phone may be offline when their lecture starts). Event-based notifications cannot — they communicate state changes made by other users.

## Contents

- `docs/product/06-notifications/00-overview.md` — the eight-type taxonomy in one place.
- `docs/product/06-notifications/01-time-based.md` — types 1–5: lecture upcoming, assignment deadline, CAT, exam, school deadline.
- `docs/product/06-notifications/02-event-based.md` — types 6–8: shared-resource change, resource shared with you, approval-pending.
