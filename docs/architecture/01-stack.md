This file covers the chosen technologies for each layer of Mimir with a one-line rationale per choice.

## Stack summary

| Layer | Choice | Why |
|---|---|---|
| Client language | Dart / Flutter | Single codebase across web, iOS, and Android. |
| Client state | `hooks_riverpod` + `flutter_hooks` | Composable, testable, no inheritance traps. |
| Client routing | `go_router` | Declarative deep-linking; web URL parity. |
| Client network | Ferry | GraphQL-native, codegen-driven, persisted cache built-in. |
| Client cache | Hive (via Ferry) | Cross-platform key-value store; small enough for web. |
| Client notifications (local) | `flutter_local_notifications` | OS-scheduled alarms for types 1–5. |
| Client notifications (push) | `firebase_messaging` | FCM data-only messages for types 6–8. |
| Client telemetry | Sentry | Crashes + breadcrumbs; trace-ID-tagged. |
| Server language | Rust | Predictable performance, strong contracts, low ops cost. |
| Server HTTP | Axum | Tower-based, ergonomic, plays well with `async-graphql`. |
| Server GraphQL | `async-graphql` | Relay-style cursors and `Connection` types out of the box. |
| Server DB driver | `neo4rs` | Native Bolt driver for Neo4j. |
| Database | Neo4j AuraDB | Relationships are first-class in this domain. See `docs/architecture/06-database/00-overview.md`. |
| Object storage | Tigris on Fly | S3-compatible, co-located with the server region. |
| Push | FCM | Single transport for Android, iOS via APNs, and web via service worker. |
| Server telemetry | OpenTelemetry → Axiom | Vendor-neutral SDK; Axiom for query and retention. |
| Hosting | Fly.io (`jnb`) | Proximity to Kenyan users; simple release model. |
| CI/CD | GitHub Actions + Fly auto-deploy | Lint, test, SDL-drift on PR; deploy on merge to `main`. |

Each row is expanded in the appropriate sub-folder. Library versions are deliberately omitted here — they live in lockfiles, not docs.
