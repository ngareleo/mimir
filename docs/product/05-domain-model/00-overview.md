This file covers the modelling principles and shared vocabulary for Mimir's domain.

**Graph not relational.** Every domain concept is a **node**. Every association is a **directed, named relationship**. There are no join tables; sharing, ownership, and approval are first-class relationships rather than rows. The original prototype's `SharedOwnership` join table becomes a labelled relationship between a user and a resource.

**Resource.** A umbrella term for the things a student creates and can share: timetables and semesters (top-level shareable), and the items that live inside them (units, lectures, tasks, events, assignments, papers). The term is used throughout the codebase and in roles ("resource owner", "shared resource").

**Owner.** The user node connected to a resource by an `OWNS` relationship. Set at creation and immutable.

**Admin.** Synonym for the owner of a **shared** resource. The role exists only once a resource is shared; before sharing, the creator is simply the owner.

**Moderator.** A user appointed by an admin via an explicit relationship on the shared resource. See `docs/product/05-domain-model/10-shared-ownership.md`.

**Approver.** The user (admin or moderator) who approved a change to a shared item. Stored as a property on items that carry an approval state (assignments, papers, lectures, etc.).

**Properties listed in each file are illustrative**, drawn from the paper's ERD (Fig 6 p.28, Fig 17 p.39) and the rebuild's role taxonomy. The committed GraphQL SDL in `packages/graphql-schema` is the source of truth.

**Relationships use upper-snake-case** verbs (e.g. `OWNS`, `BELONGS_TO`, `MODERATES`, `LINKED_TO`). Nodes use PascalCase labels.

For relationship-level conventions and ID strategy, see `docs/architecture/07-data-model/01-naming-and-ids.md`.
