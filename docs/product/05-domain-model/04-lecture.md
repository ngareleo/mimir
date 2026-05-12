This file covers the Lecture node.

A **Lecture** is one recurring class slot for a unit, placed on a timetable. Lectures are the source of type-1 lecture-upcoming notifications (see `docs/product/06-notifications/01-time-based.md`).

**Properties.**

- `id` — opaque global identifier.
- `dayOfWeek` — Monday through Sunday. Lectures recur weekly within the parent timetable's date range.
- `timeFrom`, `timeTo` — start and end times of the class.
- `venue` — physical or virtual location.
- `dateCreated` — audit timestamp.

**Relationships.**

- `(:Unit)-[:HAS_LECTURE]->(:Lecture)` — the unit being taught. Exactly one.
- `(:Timetable)-[:CONTAINS]->(:Lecture)` — the timetable this slot sits on. Exactly one. Lectures inherit visibility and sharing from this timetable.
- `(:User)-[:APPROVED]->(:Lecture)` — set when a non-owner's edit to a lecture on a shared timetable is approved. Tracks who approved.

**Recurrence model.** A lecture recurs weekly between the parent timetable's effective range (which usually mirrors a semester's start and end dates). There is no per-lecture end date; a lecture is removed by deleting the node.

**Sharing.** A lecture is not shared on its own. It moves with its containing timetable. When a member of a shared timetable edits a lecture, the change enters the approval flow (see `docs/product/04-use-cases/03-approving-changes.md`).

Lectures supply the data shown on the Home screen's "Today's lectures" list (see `docs/design/03-home/01-populated.md`).
