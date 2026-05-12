This file covers the User node.

A **User** represents one Mimir account. Every interactive node in the graph traces back to a user via at least one relationship (`OWNS`, `SHARED_WITH`, `MODERATES`, or `CREATED`).

**Properties.**

- `id` — opaque global identifier.
- `email` — primary login identifier.
- `phoneNumber` — collected at registration; used for identification, not for messaging in v1.
- `firstName`, `lastName`, `username` — display identity.
- `dateAdded` — account creation timestamp.

Authentication credentials (password hash, OAuth subject) are kept out of the domain view; they live with the auth subsystem (see `docs/architecture/08-authentication/index.md`).

**Relationships the user participates in.**

- `(:User)-[:OWNS]->(:Semester | :Timetable | :Unit | :Lecture | :Task | :Event | :Assignment | :Paper)` — the user created the resource.
- `(:User)-[:SHARED_WITH]->(:Semester | :Timetable)` — the user is a member of a shared resource. The relationship carries the role (admin, moderator, member) and the appointer reference for moderators.
- `(:User)-[:MODERATES]->(:Semester | :Timetable)` — convenience traversal for moderator listings; equivalent to a `SHARED_WITH` with role moderator.
- `(:User)-[:APPROVED]->(:Assignment | :Paper | :Lecture | …)` — the user approved a change request on the item.

A single user node carries all of these relationships simultaneously; roles are properties of the relationship, not states on the user.

For the role taxonomy itself, see `docs/product/02-users.md`. For the share-relationship payload, see `docs/product/05-domain-model/10-shared-ownership.md`.
