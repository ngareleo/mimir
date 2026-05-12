This file covers the `:Assignment` and `:Paper` labels.

## `:Assignment`

An assignment is a piece of coursework attached to a unit. Properties:

- `id` — UUIDv7.
- `title` — short text.
- `description` — optional free-text.
- `dueAt` — ISO-8601 UTC.
- `submissionUrl` — optional Tigris object key for an uploaded artefact. See `docs/architecture/13-file-storage/01-presigned-upload-flow.md`.
- `createdAt`, `updatedAt`.

### Edges

- Incoming: `(:Unit)-[:CONTAINS]->(:Assignment)` — composition.
- Incoming optional: `(:Task)-[:LINKED_TO]->(:Assignment)` — a task tracking submission progress.
- Incoming optional: `(:Event)-[:LINKED_TO]->(:Assignment)` — a calendar reminder for the due window.

## `:Paper`

A paper is a past exam or CAT past-paper attached to a unit. Properties:

- `id` — UUIDv7.
- `title` — display label.
- `year` — integer.
- `kind` — `CAT` | `EXAM`.
- `fileUrl` — Tigris object key.
- `createdAt`, `updatedAt`.

### Edges

- Incoming: `(:Unit)-[:CONTAINS]->(:Paper)` — composition.
- Incoming optional: `(:Task)-[:LINKED_TO]->(:Paper)` — a task referencing a paper to study.

## Why both live under a unit

Both belong to the academic structure of a unit, not to a single user. They are shared with whoever has access to the parent semester, and their lifecycle follows the unit's.

## File ownership

Both labels reference Tigris objects by key. The server records the key on the node when `completeUpload` runs; before that, the key is null. Uploads are presigned per request — the node holds the key, not credentials.
