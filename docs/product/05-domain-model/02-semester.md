This file covers the Semester node.

A **Semester** is the top-level container for a student's term: a bounded period that groups the units they are taking, their class timetable, and the academic items (assignments, CATs, papers, tasks, events) tied to those units. It is one of the two shareable resource types (the other is Timetable).

**Properties.**

- `id` — opaque global identifier.
- `name` — e.g. "Computer Science 2022/23 Semester 2".
- `startDate`, `endDate` — the period boundaries. Drive notification scheduling for items inside the semester.
- `dateCreated`, `dateLastModified` — audit timestamps.

**Relationships.**

- `(:User)-[:OWNS]->(:Semester)` — the creator. Exactly one.
- `(:User)-[:SHARED_WITH {role}]->(:Semester)` — zero or more shared members.
- `(:Semester)-[:CONTAINS]->(:Unit)` — units taken in this semester.
- `(:Semester)-[:HAS_TIMETABLE]->(:Timetable)` — the class timetable for the semester.
- `(:Semester)-[:CONTAINS]->(:Assignment | :Paper | :Task | :Event)` — academic items and reminders scoped to the semester.

**Sharing semantics.** When a semester is shared, every item it contains becomes visible to members. Adding a new contained item (e.g. an assignment) by a non-owner is gated through the approval flow (see `docs/product/04-use-cases/03-approving-changes.md`). Deleting the semester cascades to its contained items.

Semester is the natural broadcasting unit for a class: a course rep creates one per term and shares it with classmates, who then receive every assignment posted to it without re-entering information. See `docs/product/04-use-cases/02-sharing-resources.md`.
