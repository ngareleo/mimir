This file covers naming conventions for labels and relationships, and the ID strategy.

## Node labels

- **Singular, PascalCase.** `:User`, `:Semester`, `:Unit` — never `:Users`, never `:semester`.
- **One primary label per node.** A node carrying `:Superuser` *also* carries `:User`; otherwise nodes have one label.
- **Labels match domain vocabulary.** They mirror the entities described in `docs/product/05-domain-model/index.md`.

## Relationship labels

- **UPPER_SNAKE_CASE.** `:OWNS`, `:SCHEDULED_AT`, `:LINKED_TO`.
- **Verb-shaped.** Active voice. `:OWNS` from owner to owned. `:MODERATES` from moderator to moderated. `:CONTAINS` from parent to child.
- **Direction is meaningful.** Read edges left-to-right as the verb suggests.

### Conventions in use

| Edge | From | To | Meaning |
|---|---|---|---|
| `:OWNS` | `:User` | `:Semester`, `:Timetable`, `:Task`, `:Event` | The owner has full authority. |
| `:MODERATES` | `:User` or `:Superuser` | shared resource | Approval rights over changes. |
| `:CONTAINS` | parent | child | Hierarchical composition (Semester→Unit→Lecture). |
| `:SCHEDULED_AT` | `:Timetable` | `:Lecture` | Carries `dayOfWeek`, `startTime`, `endTime`. |
| `:LINKED_TO` | `:Task` or `:Event` | any resource | Polymorphic association. |
| `:MEMBER_OF` | `:User` | `:SharedOwnership` | Membership in a shared resource. |
| `:SHARED_VIA` | resource | `:ShareableLink` | Public-link sharing. |
| `:ENROLLED_IN` | `:User` | `:Semester` | A non-owner participant in a semester. |

## ID strategy

Every node carries an opaque global `id` as a string property. The format is **UUIDv7** — time-ordered, monotonic, 128-bit, no PII. UUIDv7 was chosen over UUIDv4 for index locality on time-sorted reads and over Hashids because Hashids reverse to integers, which we do not have a natural source for in a graph database. Full reasoning lives in `docs/architecture/16-graphql-conventions/04-ids.md`.

IDs are opaque on the wire. The client treats them as strings.
