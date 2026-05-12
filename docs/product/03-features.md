This file covers the functional capabilities Mimir provides to a student.

The feature surface is small and deliberately opinionated. Each capability exists to remove a manual step a student would otherwise perform inside a WhatsApp group or a calendar app.

- **Create and maintain a timetable.** A weekly schedule of lectures with day, start and end time, and venue. A timetable can stand alone or attach to a semester.
- **Create and maintain a semester.** A bounded period (start date, end date) that groups the student's units, their timetable, and academic items for the term.
- **Create units and lectures.** A unit (e.g. SMA200 Calculus 2) has lectures placed on a timetable. Tasks and academic items can attach to a unit so notifications carry the unit name.
- **Create tasks and events.** A task is a one-off item to be reminded of; an event is a scheduled occurrence with a venue. Both can be linked to a semester, a unit, or a timetable.
- **Create academic items: assignments, CATs, papers (exams).** These are typed reminders for school work with deadlines, dates, and venues.
- **Share resources.** A timetable or semester can be shared with another student. Sharing makes the originator the resource Admin and grants the recipient read access.
- **Appoint and remove moderators.** Admins delegate approval rights for a shared resource.
- **Propose and approve changes.** Shared members request edits; admins or moderators approve or reject them.
- **Receive reminders and notifications.** See `docs/product/06-notifications/index.md`.

Features depend on the entities in `docs/product/05-domain-model/index.md`. UI surfaces for these features live under `docs/design/`.
