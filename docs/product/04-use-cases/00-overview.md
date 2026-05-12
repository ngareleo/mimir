This file covers the top-level use-case map for Mimir.

The paper (Fig 3, p.25) collapses everything a user does into two top-level use cases: **creating and managing time-management resources**, and **sharing those resources and managing them collectively**. The other journeys in this folder are specialisations of those two.

Every actor is a Kenyan university student. The roles they hold (Student, Admin, Moderator — see `docs/product/02-users.md`) are scoped per resource, so the same account can be an admin for one semester and an ordinary member of another.

**Creating and managing.** A student creates personal resources — timetables, units, lectures, tasks, events, assignments, CATs, papers, semesters — by filling out forms in the app. They can edit and delete what they own. This path requires only that the user be authenticated and have the information they want to record. It is the entry point for every new user.

**Sharing and managing collectively.** A student who owns a resource shares it with another Mimir user. The owner becomes the resource's Admin. The recipient can read the resource, link their own tasks to it, and propose changes that go through approval. The admin can appoint moderators who help approve those changes, and can remove members who should no longer have access.

The sub-files break this down:

- Creating personal resources — `docs/product/04-use-cases/01-creating-resources.md`
- Sharing a resource with another student — `docs/product/04-use-cases/02-sharing-resources.md`
- Approving change requests on a shared resource — `docs/product/04-use-cases/03-approving-changes.md`
- Admin actions on a shared resource — `docs/product/04-use-cases/04-moderation.md`
