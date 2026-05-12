This file covers the GitHub Actions workflows that gate PRs and deploy to Fly.

## Workflows

Three workflows in `.github/workflows/`:

- **`ci.yml`** — runs on every PR. Steps: cargo lint (`cargo fmt --check`, `cargo clippy --deny warnings`), Dart lint (`dart format --set-exit-if-changed`, `dart analyze`), Rust tests against a shared dev Neo4j started as a service container, client widget + golden tests via `flutter test`, and an **SDL drift check** that runs `cargo run --bin print-schema` and fails if the generated file differs from the committed `packages/graphql-schema/schema.graphql`.
- **`deploy.yml`** — runs on push to `main` after `ci.yml` passes. Builds the server Docker image, pushes to Fly's registry, and triggers `fly deploy`. Fly's `release_command` applies migrations before the new machine takes traffic; if migration fails the deploy aborts and the previous machine keeps serving.
- **`pending-upload-sweep.yml`** — scheduled hourly. Hits a server endpoint protected by a workflow-only token that runs the `:PendingUpload` cleanup query. See `docs/architecture/13-file-storage/01-presigned-upload-flow.md`.

## Secrets

GitHub Actions secrets:

- `FLY_API_TOKEN` — deploy and trigger commands.
- `SWEEP_TOKEN` — scheduled-sweep authentication header.

Per-environment Neo4j credentials live as Fly secrets, not GitHub secrets — CI uses an ephemeral Neo4j service container, not the prod database.

## Branch protection

`main` requires `ci.yml` green and at least one approving review. Force-push and direct push to `main` are disallowed; merge happens via PR.
