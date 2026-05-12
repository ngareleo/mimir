# Link to timetable

This file covers the sub-modal opened from a task or event form's **Link to timetable** row.

## Layout

The sub-modal stacks on top of the parent form. From top to bottom:

- A title: **Link to timetable**.
- **Choose timetable** — a dropdown selector whose default option is **Custom** and whose other entries are the user's timetables and the timetables of any semester or unit in scope.
- A control to **+ Pick day of week**, which opens a weekday picker.
- A row of two date controls separated by an arrow, representing the start and end dates of the link's validity window.
- A trailing pair of buttons: **Cancel** and **Add**.

## Behaviour

If a real timetable is chosen, **Pick day of week** and the date range constrain when the linked task or event should appear within that timetable's schedule. If **Custom** is selected, the day-of-week and date range act as a freestanding recurrence definition rather than binding to an existing timetable.

**Cancel** dismisses the sub-modal without recording a link; **Add** records the link and returns to the parent form, replacing the link row's placeholder text with a summary of the chosen reference. Multiple links can be set on a single task or event by reopening the row.

## Cross-references

- `docs/product/05-domain-model/05-timetable.md` for the timetable entity and its event semantics.
- `docs/design/06-forms/00-task-form.md` and `docs/design/06-forms/01-event-form.md` for the parent forms that open this sub-modal.
