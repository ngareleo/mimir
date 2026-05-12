# Populated state

This file covers the Home tab when the selected date has scheduled content.

## Layout

Below the calendar strip, the scrollable body is divided into three labelled sections, each rendered as a stack of cards. The sections appear in this order:

1. **Today's lectures** — cards for each lecture on the selected day. Each card shows a leading time hint (e.g. a relative phrase such as "In about 30 min" or an absolute time like "At 1:00PM"), the unit's label as the card title, and a secondary line with the unit code and lecturer.
2. **Assessment tests this week** — cards for upcoming CATs and exams within the current week, grouped under day-of-week subheadings (e.g. **Today**, **Thursday**).
3. **Assignments due this week** — cards for assignments whose due date falls inside the current week, grouped under day-of-week subheadings.

The FAB floats over the bottom-right of the scrolling region. The bottom navigation and app bar remain fixed.

## Behaviour

Sections with no content for the current week are omitted entirely rather than shown empty. Tapping a card opens the underlying resource — a lecture, an assessment, or an assignment — on its detail screen. Changing the active date in the calendar strip re-queries each section against the new day's context.

The week boundary used for "this week" sections is fixed by the user's locale; lectures are always day-scoped to the active date, while assessments and assignments are week-scoped to provide forward visibility.
