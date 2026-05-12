# Task form

This file covers the modal form used to create or edit a task.

## Layout

From top to bottom inside the modal:

- A leading dismiss affordance (close) and a trailing primary action: **Save**.
- **Choose the task type** — a dropdown selector with a placeholder of **Select...**. Options include assignment, personal task, and the other task types enumerated in `docs/product/05-domain-model/06-task.md`.
- **Task label** — a single-line free-text field.
- **Task description** — a multi-line free-text field.
- A date control: **+ Pick date**, which opens the platform date picker.
- A vertical group of link rows, each prefixed with a chain affordance:
  1. **Link to unit**
  2. **Link to semester**
  3. **Link to timetable** — opens the link-to-timetable sub-modal described in `03-link-to-timetable.md`.

## Behaviour

Task type and label are required; description, date, and links are optional. Selecting a link row opens a picker scoped to the user's resources of that type; the chosen reference replaces the row's placeholder text. If the form is opened from a context that already implies a parent (e.g. the Semester tab's FAB), the corresponding link is pre-filled and shown as set.

**Save** submits the task; success dismisses the modal and refreshes the originating list. The underlying screen remains visible behind the modal throughout.
