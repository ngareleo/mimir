This file covers Mimir's deployment topology at a glance.

```
Kenyan user device
  └── Mimir client (Flutter web/iOS/Android)
        └── HTTPS over Fly anycast → Fly.io jnb region
              └── Mimir server (Rust + Axum)
                    ├── neo4rs → AuraDB Pro (eu-west)  [~150ms]
                    ├── S3 SDK → Tigris on Fly        [edge-distributed]
                    └── OTel exporter → Axiom
```

## Where things run

- **Server:** Fly.io, `jnb` region, single machine in v1. Configuration in `apps/server/fly.toml`.
- **Graph DB:** AuraDB. Two instances: Free tier for dev/staging, Pro Starter in `gcp-europe-west1` for prod. See [02-auradb.md](02-auradb.md).
- **Object storage:** Tigris on Fly. Bytes never transit the Mimir server.
- **Push:** FCM (Google). The server holds an FCM service-account credential.
- **Telemetry:** OpenTelemetry → Axiom workspace.
- **Client:** No deployment beyond app store / web hosting. iOS through TestFlight then App Store; Android through Play Console; web served alongside the Flutter web build (deployed with the same Fly app or behind a CDN — TBD when web ships).

## Latency budget

The largest hop is the server↔DB roundtrip (~150ms JNB↔eu-west). Mitigated by:

- Designing GraphQL operations to issue few Cypher queries per request.
- Leaning on Ferry's persisted cache on the client for warm reads.
- The client↔server hop stays fast at ~30ms KE↔JNB.

## Deploy mechanics

PR opens → GitHub Actions runs lint, tests, SDL-drift check. Merge to `main` → Fly auto-deploys: build, push image, `release_command` runs `cargo run --bin migrate`, then the new machine takes traffic. See [03-ci-cd.md](03-ci-cd.md).
