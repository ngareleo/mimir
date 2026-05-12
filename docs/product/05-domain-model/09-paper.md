This file covers the Paper node.

A **Paper** represents any timed academic assessment for a unit: a continuous assessment test (**CAT**), an in-semester assessment, or an end-of-semester exam. The single node carries a type discriminator so the three cases can be distinguished without three separate labels. CATs drive type-3 notifications; exams drive type-4 notifications (see `docs/product/06-notifications/01-time-based.md`).

The paper's prototype distinguished `AssessmentTest` from `Paper` in the relational ERD (Fig 6 p.28). Mimir collapses these to one node type with a `kind` property.

**Properties.**

- `id` — opaque global identifier.
- `kind` — one of `CAT`, `ASSESSMENT`, `EXAM`. Drives notification type and UI grouping.
- `title` — short name.
- `description` — optional notes.
- `date` — the date of the paper.
- `time` — start time on `date`.
- `venue` — where the paper is sat.
- `creator` — reference to the User who recorded the paper.
- `approver` — reference to the User who approved the paper when added to a shared semester by a non-owner. Null otherwise.
- `dateCreated`, `dateLastModified` — audit timestamps.

**Relationships.**

- `(:Unit)-[:HAS_PAPER]->(:Paper)` — the unit being assessed. Exactly one.
- `(:Semester)-[:CONTAINS]->(:Paper)` — derived from the unit's semester.
- `(:User)-[:CREATED]->(:Paper)` — the creator.
- `(:User)-[:APPROVED]->(:Paper)` — set on approval.

**Sharing and approval.** Identical to Assignment: non-owner additions to a shared semester are gated by the approval flow (see `docs/product/04-use-cases/03-approving-changes.md`).
