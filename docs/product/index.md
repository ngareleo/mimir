# Product

The product domain describes **what Mimir is** for Kenyan university students: the problem it addresses, who uses it, the features it offers, the domain entities it manages, and the notifications it produces. These files are reference material for engineers and AI agents working on the system — they describe the world Mimir models, not how it is built. For the implementation side, see `docs/architecture/index.md`; for screen-level UI, see `docs/design/index.md`.

## Contents

- `docs/product/00-overview.md` — one-page summary of Mimir and how its pieces fit together.
- `docs/product/01-problem.md` — the user problem (WhatsApp-as-info-bus) and why existing planners fail Kenyan students.
- `docs/product/02-users.md` — the three roles: Student, Resource Owner (Admin), and Moderator.
- `docs/product/03-features.md` — the functional capabilities the system provides.
- `docs/product/04-use-cases/` — primary user journeys grouped under one folder.
- `docs/product/05-domain-model/` — the entities Mimir stores and the relationships between them, expressed as a graph.
- `docs/product/06-notifications/` — the eight-type notification taxonomy and how time-based and event-based notifications differ.
- `docs/product/07-scope.md` — what is in and out of scope for v1.
