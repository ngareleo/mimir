# Deployment

This folder covers how Mimir is hosted, deployed, and how secrets and CI/CD glue the pieces together.

Mimir's server runs on Fly.io in the `jnb` region for low latency to Kenyan users. The graph database is AuraDB — Free for dev/staging, Pro Starter in `eu-west` for prod. CI/CD lives in GitHub Actions: PRs run lint, tests, and an SDL-drift check; merges to `main` auto-deploy to Fly.

## Direct children

- [00-overview.md](00-overview.md) — system at deploy-time: where things run and why.
- [01-fly-server.md](01-fly-server.md) — Fly machine configuration and `release_command`.
- [02-auradb.md](02-auradb.md) — AuraDB provisioning, env vars, region tradeoff.
- [03-ci-cd.md](03-ci-cd.md) — GitHub Actions workflows and the deploy trigger.
- [04-secrets.md](04-secrets.md) — what's secret, where it lives, who reads it.
