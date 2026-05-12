# Empty state

This file covers the Home tab when the selected date has nothing scheduled.

## Layout

From top to bottom, the screen presents:

1. The global app bar (menu, **Home**, bell, gear).
2. The **calendar strip** showing the current month label, paging arrows on either side, and a row of weekday letters with their corresponding day numbers. The active day is highlighted.
3. A centered illustration in the body region — a single neutral graphic intended to occupy the empty space.
4. Beneath the illustration, the line **No tasks or events today**.
5. The **floating action button** in the bottom-right corner.
6. The three-tab bottom navigation.

## Behaviour

The empty state renders whenever the user-selected day has no lectures, no due assignments, no scheduled assessments, and no personal tasks or events. Selecting a different day on the calendar strip re-evaluates the body; if any of the three populated-state sections has content for that date, the body transitions to the populated layout described in `01-populated.md`.

The FAB remains active and produces the same create menu as in the populated state (see `02-fab-menu.md`). Creating a task or event whose date matches the selected day immediately replaces the empty state with the populated layout.

## Navigation

- **Forward:** the FAB opens the create menu; the bell opens the notifications feed; the gear opens settings.
- **Tab change:** selecting Search or Library swaps to those tabs; returning to Home preserves the previously selected date.
