# Information architecture

This file covers the global navigation that frames every screen in the app.

## Bottom navigation

Three tabs, always visible on top-level screens:

- **Home** — the daily dashboard (today's lectures, this week's assessments and assignments).
- **Search** — find resources across the user's semesters, timetables, tasks, and events.
- **Library** — the user's owned and shared resources, browsable by type.

Selecting a tab swaps the entire content region. Deep screens (e.g. a specific timetable) push on top of the active tab with a back-arrow in the app bar.

## App bar

Sits at the top of every primary screen.

- **Left:** menu icon (drawer entry) on top-level screens; back arrow on pushed screens.
- **Center:** the current screen's title.
- **Right:** notifications bell and settings gear, in that order. The bell surfaces the notification feed described in `docs/product/06-notifications/index.md`.

## Floating action button

A single FAB lives on the Home tab and on the Semester detail screen. On Home it expands a menu of create actions (see `docs/design/03-home/02-fab-menu.md`). On a semester it adds a task, an assignment, or a timetable scoped to that semester.

## Tab strips within screens

Some detail screens carry their own horizontal tab strip below the app bar — for example a semester filters its contents into **All**, **Assignments**, **Timetables**, and **Approvals**. These are local to the screen and do not affect the bottom navigation.
