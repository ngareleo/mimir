This file covers the `:Unit` and `:Lecture` labels.

## `:Unit`

A unit is a single course inside a semester. Properties:

- `id` — UUIDv7.
- `code` — short identifier (e.g. "ICS 301").
- `name` — full name.
- `createdAt`, `updatedAt`.

### Edges

- Incoming: `(:Semester)-[:CONTAINS]->(:Unit)` — structural composition.
- Outgoing: `(:Unit)-[:CONTAINS]->(:Lecture)` — the lectures that constitute the unit.
- Inherits the semester's sharing context; `:Unit` does not carry its own `:SharedOwnership`.

A unit cannot exist outside a semester. Deleting a semester deletes its units (and their lectures) by application logic; we do not rely on Cypher cascade primitives.

## `:Lecture`

A lecture is a recurring teaching slot within a unit. Properties:

- `id` — UUIDv7.
- `title` — display label.
- `notes` — optional free-text.
- `createdAt`, `updatedAt`.

### Edges

- Incoming: `(:Unit)-[:CONTAINS]->(:Lecture)` — composition.
- Incoming: `(:Timetable)-[:SCHEDULED_AT {dayOfWeek, startTime, endTime}]->(:Lecture)` — scheduling lives on the edge, not on the lecture. See `docs/architecture/07-data-model/05-timetable.md`.
- Incoming: `(:Task|:Event)-[:LINKED_TO]->(:Lecture)` — tasks and events can reference a lecture.

## Why scheduling lives on the timetable edge

A lecture is conceptually a "thing taught" — its meeting times are a function of the timetable, not the lecture itself. A unit can appear in multiple timetables (e.g. a personal weekly view vs a class-wide one) with different scheduled slots. Carrying the slot on the `:SCHEDULED_AT` edge keeps the lecture stable across timetables.
