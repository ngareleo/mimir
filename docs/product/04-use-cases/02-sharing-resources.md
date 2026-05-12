This file covers how a student shares a resource with another Mimir user.

Two resource types are shareable: **timetables** and **semesters**. Tasks, events, assignments, CATs, and papers are not shared individually — they are shared by virtue of belonging to a shared semester or timetable.

The precondition is that the recipient is an existing Mimir user. Sharing happens by inviting a user (typically by email or username) from the resource detail screen. Once the invite is accepted:

- The resource becomes shared. The originating user becomes its **Admin** (the role is recorded against the resource, not the account).
- The recipient becomes a shared member with read access. They can view all items inside the resource and use them to drive their own notifications.
- The recipient can **link** their own tasks or items to the shared resource, but linking a new item that is owned by the shared resource (e.g. adding an assignment to a shared semester) creates a change request — see `docs/product/04-use-cases/03-approving-changes.md`.

When a shared resource changes (a new assignment is added by anyone, an item is edited and approved, the owner edits directly), every shared member receives an event-based notification of type 6 (see `docs/product/06-notifications/02-event-based.md`). The recipient also receives a type 7 notification on initial share.

Sharing is reversible: an admin can remove a member at any time (see `docs/product/04-use-cases/04-moderation.md`). A removed user loses access to the resource and to any items inside it that they did not own.
