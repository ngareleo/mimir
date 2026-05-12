This file covers how a student creates personal time-management resources.

The precondition is that the student is authenticated and knows the information they want to record (a class time, an assignment deadline, a semester's start and end dates). No other resource needs to exist first; the user can start with a standalone timetable, a standalone task, or a full semester.

A student can create:

- **A timetable** — a weekly schedule. It can be created on its own or as part of a semester.
- **A semester** — a bounded period (start date, end date) that acts as a container for units, a timetable, and academic items for the term.
- **A unit** — a course taken in a semester (e.g. SMA200 Calculus 2). Units carry a name, description, and lecturer.
- **A lecture** — a recurring class on a timetable, tied to a unit, with day-of-week, start and end time, and venue.
- **A task** — a one-off reminder linked optionally to a semester, unit, or timetable.
- **An event** — a scheduled occurrence with a venue and a date or time range.
- **An assignment, CAT, or paper (exam)** — typed academic items with deadlines, tied to a unit.

Editing and deleting a personal resource is unrestricted while the resource is unshared. Once shared, the creator becomes the resource Admin and the rules in `docs/product/04-use-cases/03-approving-changes.md` apply to other members' edits — but the owner can still edit directly.

The act of creating a resource also generates the relevant time-based notifications (see `docs/product/06-notifications/01-time-based.md`). Writes require connectivity (see `docs/product/07-scope.md`).
