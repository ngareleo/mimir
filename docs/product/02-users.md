This file covers the user roles Mimir recognises and what each can do.

Mimir has one user type — a Kenyan university student — and three **roles** that a student takes on relative to a specific resource. A single account holds different roles for different resources at the same time. Roles are scoped to a resource, not to the account.

**Student.** The base role. Every account is a student. A student can create personal semesters, timetables, units, lectures, tasks, events, assignments, and papers; receive their own time-based notifications; accept shared resources from other students; and propose changes to shared resources they have access to.

**Resource Owner (Admin).** The user who created a shareable resource. When the owner shares the resource with another student, the system records them as the resource's Admin. The admin has full control: editing without approval, appointing or removing moderators, removing shared members, deleting the resource. A user is an admin only for resources they own.

**Moderator.** A user appointed by an admin to help govern a shared resource. Moderators can approve or reject change requests submitted by other shared members. Moderators cannot appoint other moderators or remove members — those actions remain with the admin. Moderation is per-resource; being a moderator on one semester does not grant rights on another.

Non-owner shared members remain students with respect to that resource: they can read it and propose changes, but their edits enter the approval queue.

For the journeys these roles take, see `docs/product/04-use-cases/index.md`. For how shared ownership is modelled, see `docs/product/05-domain-model/10-shared-ownership.md`.
