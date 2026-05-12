# FAB menu

This file covers the create menu that the Home tab's floating action button exposes.

## Layout

Tapping the FAB anchors a small menu above and to the left of the button, listing four options in this order:

1. **Create semester**
2. **Create timetable**
3. **Add task**
4. **Add event**

The menu dismisses on outside tap, on selection, or on pressing the platform back gesture. The FAB itself remains visible while the menu is open.

## Behaviour

Each option routes to a creation surface:

- **Create semester** → the semester create screen (`docs/design/05-semester/00-create.md`).
- **Create timetable** → the timetable create screen (`docs/design/04-timetable/00-create.md`).
- **Add task** → the task form modal (`docs/design/06-forms/00-task-form.md`).
- **Add event** → the event form modal (`docs/design/06-forms/01-event-form.md`).

The forms reached from **Add task** and **Add event** are the same generic forms used elsewhere in the app; they preselect no parent resource when opened from Home, so the user must choose a unit, semester, or timetable to link to via the form's link-to controls (`docs/design/06-forms/03-link-to-timetable.md`).

## Cross-references

- `docs/product/05-domain-model/02-semester.md`, `docs/product/05-domain-model/05-timetable.md`, `docs/product/05-domain-model/06-task.md`, and `docs/product/05-domain-model/07-event.md` for the entities these actions create.
