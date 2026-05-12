# Create

This file covers the screen used to create a new timetable.

## Layout

- Global app bar with a back arrow and the title **Timetable**.
- A single large free-text field used as the timetable label, with placeholder text **Timetable label...**.
- A smaller free-text field beneath the label, used for a short description, with placeholder text **A quick description...**.
- A row of two date pickers, side by side: **Pick start date** and **Pick end date**.
- A primary action button: **Create timetable**.
- The three-tab bottom navigation persists at the foot of the screen.

The screen has no FAB.

## Behaviour

The label is required; the description and the two dates are optional. If both dates are provided, the end date must not precede the start date; a violation surfaces inline. On **Create timetable**, the form submits and the user is routed to the new timetable's detail view (`01-view.md`) with the timetable in its empty state.

This screen is reachable from two entry points:

- The Home FAB's **Create timetable** option (`docs/design/03-home/02-fab-menu.md`).
- The Semester detail screen's FAB while the **Timetables** tab is active (`docs/design/05-semester/01-tabs.md`); in that case the new timetable is created as a child of the semester in scope.

## Cross-references

- `docs/product/05-domain-model/05-timetable.md` for the underlying entity and its fields.
