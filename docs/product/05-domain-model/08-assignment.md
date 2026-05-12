This file covers the Assignment node.

An **Assignment** is a typed academic deliverable owed by a student against a unit — a piece of coursework with a deadline. It is one of the three typed academic items (the others are Paper and CAT, both represented by the Paper node in v1). Assignments produce type-2 deadline notifications (see `docs/product/06-notifications/01-time-based.md`).

**Properties.**

- `id` — opaque global identifier.
- `title` — short name, e.g. "Simulation and Modelling Lab 3".
- `description` — instructions or notes.
- `dueDate` — the submission deadline.
- `creator` — reference to the User who created the assignment record. Distinct from the owner of the parent semester.
- `approver` — reference to the User (admin or moderator) who approved the assignment when added to a shared semester by a non-owner. Null until approved; null also when the creator is the owner.
- `dateCreated`, `dateLastModified` — audit timestamps.

**Relationships.**

- `(:Unit)-[:HAS_ASSIGNMENT]->(:Assignment)` — the unit this assignment is for. Exactly one.
- `(:Semester)-[:CONTAINS]->(:Assignment)` — the containing semester. Exactly one (derived from the unit's semester).
- `(:User)-[:CREATED]->(:Assignment)` — the user who recorded it. Matches the `creator` property.
- `(:User)-[:APPROVED]->(:Assignment)` — set on approval. Matches the `approver` property.

**Sharing and approval.** Adding an assignment to a shared semester by a non-owner creates a change request (see `docs/product/04-use-cases/03-approving-changes.md`). Once approved, the assignment is visible to every member and drives a type-2 notification for each. Direct edits by the admin or a moderator apply immediately.
