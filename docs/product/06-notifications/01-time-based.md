This file covers types 1–5: notifications that fire locally on the device at a known future time.

These notifications share three properties: each refers to a single domain entity owned by or shared with the user; each has a deterministic fire time computable from that entity's properties; and each must fire whether or not the device is connected. Reliability is the reason for local scheduling — a student waiting for their morning lecture may be off Wi-Fi and out of mobile credit.

**Type 1 — Lecture upcoming.** Fires before a `Lecture`'s `timeFrom`. Sourced from every lecture in every timetable the user owns or is shared on. Recurrent: rescheduled weekly within the parent timetable's effective range.

**Type 2 — Assignment deadline.** Fires before an `Assignment`'s `dueDate`. Sourced from every assignment in every semester the user owns or is shared on. Non-recurrent.

**Type 3 — CAT / assessment test.** Fires before a `Paper` with `kind` `CAT` or `ASSESSMENT`. Sourced from papers in semesters the user owns or is shared on. Non-recurrent.

**Type 4 — Exam / paper.** Fires before a `Paper` with `kind` `EXAM`. Otherwise identical to type 3; distinguished because exams are presented differently in the UI and may warrant earlier reminders.

**Type 5 — School deadline.** Fires before a user-defined deadline that is not coursework — fee payment, unit registration, or any task the user flags as a school deadline. Sourced from `Task` entities marked as school deadlines.

The lead time for each type (how far ahead to fire) is a client setting, not part of the entity. The scheduling primitive, the iOS 64-pending cap, and the Hive box that holds pending schedules are all transport concerns; see `docs/architecture/09-notifications/01-local-scheduling.md`.
