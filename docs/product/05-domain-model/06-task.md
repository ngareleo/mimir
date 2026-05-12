This file covers the Task node.

A **Task** is a one-off reminder. It is the catch-all entity for items that do not fit the typed academic shapes (Assignment, Paper, Event): "submit medical form", "pay hostel deposit", "collect transcript". Tasks are personal by default; they become visible to others when linked to a shared resource.

**Properties.**

- `id` — opaque global identifier.
- `label` — short title.
- `description` — optional longer text.
- `date` — the date the task is due or planned for.
- `from`, `to` — optional time range on `date`.
- `repeat`, `repeatUpTo` — optional recurrence configuration for tasks that repeat (e.g. weekly study sessions).
- `dateCreated`, `dateLastModified` — audit timestamps.

**Relationships.**

- `(:User)-[:OWNS]->(:Task)` — the creator. Exactly one.
- `(:Task)-[:LINKED_TO]->(:Semester | :Unit | :Timetable)` — optional links. A task can be standalone or attached to any combination.
- `(:User)-[:APPROVED]->(:Task)` — set when a non-owner adds a task to a shared semester and the change is approved.

**Sharing.** Tasks are not directly shareable. A task becomes visible to other users when it is linked to a shared semester or shared timetable. A shared member adding such a link triggers the approval flow (see `docs/product/04-use-cases/03-approving-changes.md`); the owner doing so applies immediately.

Tasks generate type-1 through type-5 notifications depending on what they're linked to and any type annotation supplied at creation. The notification taxonomy lives in `docs/product/06-notifications/01-time-based.md`.
