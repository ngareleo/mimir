This file covers the Timetable node.

A **Timetable** is a weekly schedule of lectures. It is one of the two shareable resource types (the other is Semester). A timetable can stand alone (e.g. a study group's revision schedule) or attach to a semester as that semester's class timetable.

**Properties.**

- `id` — opaque global identifier.
- `label` — human name, e.g. "Exam Timetable" or "Computer Science 2022/23".
- `description` — optional summary.
- `dateCreated`, `dateLastModified` — audit timestamps.

The timetable does not carry its own start and end dates in v1; recurrence boundaries come from the attached semester when one is present, and from explicit per-lecture configuration otherwise.

**Relationships.**

- `(:User)-[:OWNS]->(:Timetable)` — the creator. Exactly one.
- `(:User)-[:SHARED_WITH {role}]->(:Timetable)` — zero or more shared members.
- `(:Timetable)-[:CONTAINS]->(:Lecture)` — the weekly class slots.
- `(:Semester)-[:HAS_TIMETABLE]->(:Timetable)` — optional. A semester has at most one timetable; a timetable belongs to at most one semester.
- `(:Task)-[:LINKED_TO]->(:Timetable)` and `(:Event)-[:LINKED_TO]->(:Timetable)` — tasks and events that should appear alongside this timetable's view.

**Sharing semantics.** Sharing a timetable shares all its lectures. A non-owner's edit to a contained lecture goes through approval (see `docs/product/04-use-cases/03-approving-changes.md`). Sharing a semester implicitly grants visibility of its attached timetable.

Timetables drive the recurring lecture notifications described in `docs/product/06-notifications/01-time-based.md`.
