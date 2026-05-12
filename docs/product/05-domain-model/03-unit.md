This file covers the Unit node.

A **Unit** is a single course a student takes in a semester — for example, SMA200 Calculus 2 at Kenyatta University. Units bind lectures, assignments, CATs, and papers to a named course so notifications can carry the unit's name and code.

**Properties.**

- `id` — opaque global identifier.
- `code` — institutional code, e.g. "SMA200". Free text; Mimir does not validate against an institutional registry.
- `name` — human name, e.g. "Calculus 2".
- `description` — optional notes about the unit.
- `lecturer` — display name of the lecturer who teaches the unit. Free text; not a reference to a User node in v1.
- `dateCreated` — audit timestamp.

**Relationships.**

- `(:Semester)-[:CONTAINS]->(:Unit)` — the semester this unit belongs to. Exactly one.
- `(:Unit)-[:HAS_LECTURE]->(:Lecture)` — recurring class slots on a timetable.
- `(:Unit)-[:HAS_ASSIGNMENT]->(:Assignment)` — coursework tied to the unit.
- `(:Unit)-[:HAS_PAPER]->(:Paper)` — CATs, assessments, and end-of-semester exams for the unit.
- `(:User)-[:OWNS]->(:Unit)` — derived from the owning semester; recorded explicitly so units can carry approval state when their semester is shared.

A unit does not exist outside a semester in v1; there is no "global" unit catalogue. Two students who share a semester see the same unit node; two students with their own semesters for the same real-world course have two unrelated unit nodes.
