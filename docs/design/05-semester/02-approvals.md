# Approvals

This file covers the **Approvals** tab of the semester detail view.

## Layout

The header and tab strip are unchanged from `01-tabs.md`. With **Approvals** active, the body renders a vertical list of pending-change cards. Each card is an expanded preview of a proposed change rather than a compact summary.

A card contains, from top to bottom:

- A close affordance on the leading side and two trailing buttons: **Close** and **Approve**.
- **Choose the task type** — the type the change proposes (e.g. Assignment), shown as a selector control in read-only form.
- **Task label** — the proposed label text.
- **Task description** — the proposed description block.
- A date pill showing the proposed due date.
- Link rows beneath the date, prefixed with a chain affordance, summarising the proposed parent links (e.g. **Linked to SCO303**, **Link to Computer science 2022/23**, **Not linked to a timetable**).

The three-tab bottom navigation persists at the foot of the screen.

## Behaviour

**Close** dismisses the change without applying it. **Approve** applies the change to the underlying resource and removes the card from the queue. The Approvals tab's count badge decrements as cards are resolved.

Only moderators of a shared semester see populated approval cards; for a non-shared semester the tab is hidden. The full permission and dispatch model lives in `docs/architecture/11-sharing-and-permissions/03-approval-flow.md`. Approval-pending notifications surface in the app bar's bell, as described in `docs/product/06-notifications/02-event-based.md`.
