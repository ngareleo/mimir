This file covers the Fly.io configuration for the Rust server.

## Machine

- **Region:** `jnb` (Johannesburg). Closest Fly region to Kenya.
- **Size:** `shared-cpu-2x` with 1 GB RAM for v1. Bumped to `performance-1x` when memory or CPU watermarks justify it.
- **Count:** single machine in v1. Multi-region scale-out is a v2 concern; the GraphQL server is stateless so horizontal scale is mechanical when needed.

## `fly.toml` essentials

- `app = "mimir-server"`
- `primary_region = "jnb"`
- `[deploy] release_command = "/app/migrate"` — runs the migration binary against the production Neo4j before the new machine takes traffic. See `docs/architecture/06-database/02-migrations.md`.
- `[http_service] internal_port = 8080`, `force_https = true`, `auto_stop_machines = false` (cold-start latency from `auto_stop` is unacceptable for a request-driven app).
- Healthcheck on `GET /healthz` every 15 seconds. See `docs/architecture/04-network-and-graphql/03-rest-exceptions.md`.

## Docker image

Built from `apps/server/Dockerfile` as a two-stage build: a `cargo chef`-style dependency layer cache, then `cargo build --release --bin mimir-server --bin migrate --bin print-schema`. The runtime stage is `debian:bookworm-slim` with the three binaries copied in. Image targets `x86_64-unknown-linux-gnu` — Fly's machine architecture.

## Restart policy

Default Fly restart on crash. SIGTERM is honored with a 5-second grace period; in-flight requests are drained but spawned fan-out tasks are not (see `docs/architecture/11-sharing-and-permissions/04-fan-out.md`).
