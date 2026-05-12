This file covers admin and moderator actions on a shared resource.

Once a resource is shared, the original owner is recorded as its **Admin**. The admin is the only role with full governance powers; moderators are a delegated subset.

**Admin-only actions.**

- **Appoint a moderator.** The admin promotes any current shared member of the resource to moderator. The appointee gains the right to approve or reject change requests on that resource. There is no cap on the number of moderators.
- **Remove a moderator.** The admin demotes a moderator back to a regular shared member. The user retains read access to the resource.
- **Remove a shared member.** The admin revokes a user's access entirely. The removed user loses read access to the resource and to items inside it that they did not personally create.
- **Delete the shared resource.** Cascades to items owned by the resource (see `docs/product/05-domain-model/10-shared-ownership.md`).

**Moderator actions.** A moderator can approve or reject change requests on the resource they moderate (see `docs/product/04-use-cases/03-approving-changes.md`). They may also edit the resource directly. They cannot appoint other moderators, remove members, or delete the resource.

**Scope.** Both roles are per-resource. A user who is an admin of Semester A and a moderator of Semester B holds neither role on Semester C. Roles are stored on the shared-ownership relationship, not on the user account — see `docs/product/05-domain-model/10-shared-ownership.md`.

Administrative actions take effect immediately and require connectivity (see `docs/product/07-scope.md`).
