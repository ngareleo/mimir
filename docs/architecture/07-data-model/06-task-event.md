This file covers the `:Task` and `:Event` labels.

## `:Task`

A task is a personal to-do item with an optional due date. Properties:

- `id` — UUIDv7.
- `title` — short text.
- `description` — optional free-text.
- `dueAt` — optional ISO-8601 UTC; absent means undated.
- `status` — `OPEN` | `DONE`.
- `createdAt`, `updatedAt`.

### Edges

- Incoming: `(:User)-[:OWNS]->(:Task)` — the owner.
- Outgoing optional: `(:Task)-[:LINKED_TO]->(:Lecture | :Unit | :Assignment | :Paper)` — polymorphic association.

## `:Event`

An event is a one-off time-pinned occurrence. Properties:

- `id` — UUIDv7.
- `title` — short text.
- `description` — optional free-text.
- `startsAt`, `endsAt` — ISO-8601 UTC.
- `location` — optional string.
- `createdAt`, `updatedAt`.

### Edges

- Incoming: `(:User)-[:OWNS]->(:Event)` — the owner.
- Outgoing optional: `(:Event)-[:LINKED_TO]->(:Lecture | :Unit | :Assignment | :Paper)` — polymorphic association.

## Polymorphic linking

Both `:Task` and `:Event` use a single `[:LINKED_TO]` edge label to reference any kind of resource. The shape of the target is read from its label at query time. This avoids a fan of `[:LINKED_TO_LECTURE]`, `[:LINKED_TO_ASSIGNMENT]` edge types and keeps GraphQL resolvers uniform.

## Why two labels, not one

Tasks and events are operationally different:

- Tasks have a binary completion state and an optional date.
- Events have a fixed window and no completion state.

The product surfaces them differently (lists vs calendars) and notifications fire on different fields (`dueAt` for tasks, `startsAt` for events; types 1 and 2 in `docs/product/06-notifications/index.md`).
