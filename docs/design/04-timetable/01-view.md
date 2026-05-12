# View

This file covers the detail view for an existing timetable, in both its empty and populated states.

## Layout — header

A banner sits below the app bar, spanning the screen width. It contains:

- The timetable label as a prominent heading.
- A secondary line showing the owner's handle.
- A trailing edit affordance that opens the management surface (`02-share-and-manage.md`).

## Layout — empty state

When the timetable has no events:

- A centered line: **No events. Add something**.
- A wide primary button: **+ Add event**.

## Layout — populated state

When the timetable has events, the body is a vertical, scrollable list grouped by day of the week. For each group:

- A day heading (e.g. **Monday**, **Tuesday**).
- Beneath the heading, one card per event. Each card shows the event's time range as a leading hint, the event label as the card title, and a trailing overflow affordance for per-event actions.

A wide primary button **+ Add event** sits at the foot of the list.

## Behaviour

Tapping **+ Add event** opens the event form (`docs/design/06-forms/01-event-form.md`) pre-linked to this timetable. Tapping an event card opens its detail; the overflow affordance on a card exposes edit and delete actions. Tapping the header's edit affordance opens the management screen described in `02-share-and-manage.md`.
