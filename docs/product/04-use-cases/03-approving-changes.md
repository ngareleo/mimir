This file covers the approval flow for changes to a shared resource.

The flow exists because shared resources have many readers and few authoritative writers. Without approval, any member could edit a shared timetable and degrade its trustworthiness for everyone else.

**Who can edit without approval.** The resource Admin (the original owner) and any appointed Moderators can edit a shared resource directly. Their edits apply immediately and trigger a type-6 shared-resource-change notification to every member (see `docs/product/06-notifications/02-event-based.md`).

**Who needs approval.** Any non-owner, non-moderator shared member. When such a user edits a shared resource — for example, adds an assignment to a shared semester, or changes a lecture venue on a shared timetable — the edit is recorded as a **change request** instead of being applied.

**What happens to a change request.**

1. The change request enters an approval queue scoped to the resource.
2. The Admin and every Moderator on that resource receive a type-8 approval-pending notification.
3. An admin or moderator opens the approvals tab on the resource, reviews the proposed change, and either approves or rejects it.
4. On approval, the change is applied as if the admin had made it. A type-6 notification fires to all members.
5. On rejection, the change is discarded and no further notification is sent.

A user can have at most one pending change per item at a time; submitting a new edit on an item with a pending change replaces the previous request.

This flow is recorded as `approver` and approval fields on the affected entities — see `docs/product/05-domain-model/08-assignment.md` for the data carried.
