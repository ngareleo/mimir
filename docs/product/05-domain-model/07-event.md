This file covers the Event node.

An **Event** is a scheduled occurrence with a venue and a time range. It differs from a Task in that it always has a physical or logical location and a start–end time, and it is presented on the calendar surface rather than the to-do surface (see `docs/design/03-home/01-populated.md`).

Use cases include guest lectures, study group meetings, club events, and consultations with a lecturer outside the regular timetable.

**Properties.**

- `id` — opaque global identifier.
- `label` — short title.
- `description` — optional longer text.
- `venue` — where the event takes place.
- `date` — the event's date.
- `from`, `to` — start and end times on `date`.
- `repeat`, `repeatUpTo` — optional recurrence configuration.
- `dateCreated`, `dateLastModified` — audit timestamps.

**Relationships.**

- `(:User)-[:OWNS]->(:Event)` — the creator. Exactly one.
- `(:Event)-[:LINKED_TO]->(:Semester | :Unit | :Timetable)` — optional links. An event can be standalone, attached to a unit, included in a semester, or pinned to a timetable.
- `(:User)-[:APPROVED]->(:Event)` — set when a non-owner adds an event to a shared resource and the change is approved.

**Sharing.** Like Task, Event is not directly shareable. Visibility flows through links to shared semesters or timetables, and a non-owner adding the link goes through approval (see `docs/product/04-use-cases/03-approving-changes.md`).

Events fire time-based notifications driven by the `from` time (see `docs/product/06-notifications/01-time-based.md`).
