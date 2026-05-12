# Filters

This file covers the multi-select chip filter used by list surfaces.

## Layout

The filter renders as a compact panel anchored to its trigger control. It contains a vertical list of checkable rows, each row a labelled checkbox. The list always begins with a single **All** row that toggles every other row in lockstep; the remaining rows are the items in scope.

Two filter contexts appear in the mockup set:

- A unit filter inside a semester, listing **All** followed by the unit names enrolled in that semester (e.g. *Operating systems*, *Computer Networks*, *Simulation and modelling*, *Entrepreneurship*, *Automata theory*, *System analysis and design*).
- A semester filter, listing **All** followed by the user's semesters (e.g. *2023/23 Semester 2*).

A small count badge on the trigger reflects the current selection.

## Behaviour

Selections are committed live: toggling a row updates the underlying list immediately, with no separate apply step. Toggling **All** when any subset is selected selects every row; toggling **All** when everything is selected clears the selection back to none. Tapping outside the panel dismisses it without altering selections.

A filter's set of rows is determined by the surface that opens it — for example, a semester's filter exposes that semester's units, while a Library filter would expose the user's semesters.

## Cross-references

- `docs/product/05-domain-model/03-unit.md` for the unit entity that backs the unit filter.
- `docs/product/05-domain-model/02-semester.md` for the semester entity that backs the semester filter.
