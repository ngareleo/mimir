# Tabs

This file covers the semester detail view and the local tab strip that filters its contents.

## Layout — header

A banner sits below the app bar, spanning the screen width. It contains:

- A small avatar.
- The semester label as a prominent heading.
- A secondary line showing the owner's handle.
- A trailing share affordance.

## Layout — tab strip

Directly beneath the header, a scrollable horizontal tab strip. Each tab shows its name and a numeric count badge:

- **All** — every assignment, timetable, and pending approval in the semester.
- **Assignments** — assignment cards only.
- **Timetables** — timetable cards only.
- **Approvals** — pending proposed changes (see `02-approvals.md`).
- Additional content-type tabs may scroll into view to the right.

## Layout — body and FAB

The body renders a vertical list of cards filtered by the active tab. Each card carries a small type badge in a top corner (e.g. **Assignment**), the resource label, and trailing metadata such as a relative-time hint.

A **floating action button** in the bottom-right of the body expands the same create menu as elsewhere, scoped to this semester: add an assignment, add a timetable, or add a task linked to the semester.

## Behaviour

Switching tabs re-filters the card list. Tapping a card opens that resource's detail screen; the FAB's options route to the relevant create flows in `docs/design/06-forms/index.md` and `docs/design/04-timetable/00-create.md`, each pre-linked to the current semester.
