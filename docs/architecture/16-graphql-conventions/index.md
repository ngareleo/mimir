# GraphQL conventions

This folder covers the schema conventions Mimir's GraphQL API follows. They keep the schema consistent across resolvers and predictable for client codegen. Conventions documented here are checked by code review; some are also enforced by the SDL-drift CI check.

## Direct children

- [00-overview.md](00-overview.md) — what these conventions exist for and how they're enforced.
- [01-pagination.md](01-pagination.md) — Relay-style cursor pagination for all list fields.
- [02-errors.md](02-errors.md) — error extensions and the categories Mimir surfaces.
- [03-naming.md](03-naming.md) — naming rules for types, fields, mutations, inputs, and enums.
- [04-ids.md](04-ids.md) — opaque global ID strategy, encoding, and resolution.
