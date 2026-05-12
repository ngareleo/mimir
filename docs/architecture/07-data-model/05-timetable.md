This file covers the `:Timetable` label and how it schedules lectures.

## Shape

A timetable is a recurring weekly schedule that pins lectures to days and times. Properties:

- `id` — UUIDv7.
- `name` — display label.
- `effectiveFrom`, `effectiveUntil` — calendar dates that bound the recurrence.
- `createdAt`, `updatedAt`.

## Edges

- Incoming: `(:User)-[:OWNS]->(:Timetable)` — the owner.
- Outgoing: `(:Timetable)-[:SCHEDULED_AT {dayOfWeek, startTime, endTime, location?}]->(:Lecture)` — one edge per scheduled slot.
- Optional: `(:Timetable)-[:LINKED_TO]->(:SharedOwnership)` — when a timetable is shared with moderators.
- Optional: `(:Timetable)-[:SHARED_VIA]->(:ShareableLink)` — public link sharing.

## Scheduling on the edge

The slot — day of the week, start time, end time, optional location — is properties on the `:SCHEDULED_AT` edge, not on the lecture or the timetable. This is deliberate:

- Removing a lecture from a timetable is one edge delete; the lecture itself is unaffected.
- A lecture can appear in multiple timetables (e.g. a study-group's shared timetable plus the owner's personal one) with different slots.
- Querying "what is on Monday for user X" follows edges with `dayOfWeek = 'MONDAY'` directly — no auxiliary table.

## Why a timetable is a node

A timetable is a first-class object: it is named, owned, shared, and rendered as a screen. It is not a property of a semester or a unit.

## Relationship to semester

There is no direct `:Semester`→`:Timetable` edge. A timetable references its lectures, and those lectures resolve to a semester through `[:CONTAINS]`. A timetable's effective dates are the user's responsibility to align with the semester it covers; the model does not enforce alignment.
