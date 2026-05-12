This file covers the `:User` and `:Superuser` labels.

## `:User`

The primary actor. Properties:

- `id` — UUIDv7.
- `email` — unique, case-folded.
- `name` — display name.
- `passwordHash` — Argon2id; absent for users created exclusively via Google OAuth.
- `googleSubject` — Google's `sub` claim; present after first Google sign-in. Used to re-link sessions.
- `createdAt`, `updatedAt`.

Authentication detail lives in `docs/architecture/08-authentication/index.md`.

## `:Superuser`

Applied *in addition to* `:User`. A `:Superuser` has moderation rights across resources flagged for global moderation (currently used only for school-deadline-style content the team curates). A node carrying `:Superuser` always also carries `:User`; there is no standalone superuser.

## Outgoing edges

- `(:User)-[:OWNS]->(:Semester)` — the user created the semester and holds full authority.
- `(:User)-[:OWNS]->(:Timetable)` — same, for timetables.
- `(:User)-[:OWNS]->(:Task)` — same, for tasks.
- `(:User)-[:OWNS]->(:Event)` — same, for events.
- `(:User)-[:ENROLLED_IN]->(:Semester)` — non-owner participation.
- `(:User)-[:MODERATES]->(:Semester | :Timetable)` — granted moderation rights.
- `(:User)-[:MEMBER_OF]->(:SharedOwnership)` — membership in a shared resource. See `docs/architecture/07-data-model/08-sharing.md`.

## Incoming edges

None. `:User` is the root of ownership chains.

## Constraints

- `id` unique and exists.
- `email` unique.

See `docs/architecture/06-database/03-constraints-and-indexes.md`.
