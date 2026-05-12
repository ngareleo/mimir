This file covers the rationale for Mimir's GraphQL conventions and how they're enforced.

## Why conventions

Mimir's API is server-defined: the SDL emitted by `cargo run --bin print-schema` is the contract. Without conventions, every resolver author would invent slightly different shapes — `Foo` vs `Result_Foo`, `nodes` vs `items`, `BadRequest` vs `INVALID_INPUT` — and the client would burn time on idiosyncrasies. Conventions make the schema self-similar so codegen output is predictable and ergonomic.

## What's covered

- **Pagination:** every list returning more than one element uses Relay's cursor connection pattern. See [01-pagination.md](01-pagination.md).
- **Errors:** application errors are returned in `extensions` with a stable `code` and a developer-facing `message`. GraphQL-level errors (parse, validation) remain in the top-level `errors` array. See [02-errors.md](02-errors.md).
- **Naming:** types are PascalCase singular; fields and arguments are camelCase; mutation names are `verbNoun` (e.g., `createSemester`); inputs end in `Input`. See [03-naming.md](03-naming.md).
- **IDs:** every node-like type exposes a single `id: ID!` that encodes type + database identifier. See [04-ids.md](04-ids.md).

## Enforcement

- **SDL-drift CI step:** the committed `packages/graphql-schema/schema.graphql` must match what the server emits. Drift fails CI.
- **Pre-commit hook:** regenerates the SDL locally so authors see the diff before pushing.
- **Code review:** naming and pagination compliance are reviewer responsibility. There is no static linter for them yet.
