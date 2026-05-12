# Client

The Flutter client targets web, iOS, and Android from one codebase. State runs on `hooks_riverpod` with `flutter_hooks`. Network goes through Ferry against the SDL in `packages/graphql-schema`. Routing is `go_router`. The bulk of the surface is GraphQL; the only direct HTTP exchanges are the documented REST exceptions described in `docs/architecture/04-network-and-graphql/03-rest-exceptions.md`.

## Direct children

- [00-overview.md](00-overview.md) — how the client pieces fit and what each layer owns.
- [01-state-management.md](01-state-management.md) — why `hooks_riverpod` + `flutter_hooks` and the patterns we apply.
- [02-routing.md](02-routing.md) — `go_router` setup and the web URL parity contract.
- [03-platforms.md](03-platforms.md) — web/iOS/Android targets and the web parity caveats.
- [04-codegen.md](04-codegen.md) — Ferry codegen, `build_runner`, and the `freezed` boundary.
