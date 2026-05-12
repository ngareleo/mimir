This file covers naming rules for the GraphQL schema.

## Types

- **Object types:** PascalCase, singular noun. `Semester`, `Timetable`, `User`.
- **Connection types:** type name + `Connection`. `SemesterConnection`. Edge types: type name + `Edge`. `SemesterEdge`.
- **Input types:** operation name + `Input`, when scoped to a mutation; or type name + `Input` for shared inputs. `CreateSemesterInput`, `UpdateTimetableInput`.
- **Enums:** PascalCase. Values are SCREAMING_SNAKE_CASE. `enum PaperKind { CAT, ASSESSMENT, EXAM }`.

## Fields

- **Object fields:** camelCase. `startDate`, `createdAt`.
- **Booleans:** `is` / `has` / `can` prefix. `isShared`, `hasModerators`, `canEdit`.
- **Timestamps:** suffix with the unit if not obvious. `createdAt` (ISO timestamp), `expiresInSec`.
- **IDs:** plain `id`; foreign references end in `Id`. `ownerId`, `semesterId`.

## Mutations

- Verb-Noun form. `createSemester`, `shareTimetable`, `approveChangeRequest`. The verb is the action; the noun is the affected type or aggregate.
- Always take a single argument named `input` typed as a dedicated Input.
- Always return the affected type (or a thin wrapper if multiple side effects need surfacing).

## Queries

- Camel-cased noun for single fetch (`semester(id: ID!)`).
- Pluralised for lists (`semesters(...): SemesterConnection!`).
- `viewer` is the convention for the authenticated user.

## What we avoid

- Hungarian prefixes (`tSemester`).
- Trailing underscores or numeric versions in field names.
- `get` / `fetch` / `query` prefixes on queries.
