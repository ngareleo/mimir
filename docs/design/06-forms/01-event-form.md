# Event form

This file covers the modal form used to create or edit an event.

## Layout

From top to bottom inside the modal:

- A leading dismiss affordance (close) and a trailing primary action: **Save**.
- **Choose the event type** — a dropdown selector. Options cover the event types in `docs/product/05-domain-model/07-event.md` (e.g. assessment test, lecture, personal event).
- **Add a label** — a single-line free-text field.
- **Enter venue** — a single-line free-text field for the location.
- **Add description** — a multi-line free-text field.
- A time-range row: two time controls separated by an arrow, representing the start and end times.
- A date control: **+ Pick date**, which opens the platform date picker.
- A vertical group of link rows, each prefixed with a chain affordance:
  1. **Link to unit**
  2. **Link to semester**
  3. **Link to timetable** — opens the link-to-timetable sub-modal described in `03-link-to-timetable.md`.

## Behaviour

Event type, label, and the time range are required; venue, description, date, and links are optional. The end time must not precede the start time; violations surface inline. As with the task form, link rows accept references scoped to the user's resources, and a known parent context pre-fills the relevant link.

**Save** submits the event; success dismisses the modal and refreshes the originating list. The underlying screen remains visible behind the modal throughout.
