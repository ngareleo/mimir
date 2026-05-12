This file covers how state is managed in the Flutter client.

## Choice

`hooks_riverpod` provides the dependency-injection and reactivity layer; `flutter_hooks` provides local widget state (`useState`, `useEffect`, `useMemoized`). They compose cleanly: providers expose application state, hooks express ephemeral widget state.

## Why this combination

- **No `BuildContext` lookup leaks.** Providers are referenced through `ref`, not `InheritedWidget` traversal.
- **Testable without the widget tree.** Provider overrides isolate logic from UI.
- **Codegen-friendly.** The `riverpod_generator` annotations pair with Ferry's generated operations cleanly.
- **Hooks reduce boilerplate** for animations, controllers, and effect cleanup, where `StatefulWidget` would otherwise dominate.

## Patterns we apply

- One provider per logical concern. Ferry query results are exposed through a `StreamProvider` (or generated equivalent) that listens to the Ferry `Client`.
- Mutations are exposed as provider methods that return a typed result; the widget surfaces success/error through hooks-driven local state.
- Auth status is a single `AuthStateProvider`. Routing and the network link both depend on it; sign-out invalidates it and Ferry's cache.
- Connectivity status is its own provider; the offline-write gate (see `docs/architecture/10-offline-and-sync/02-write-policy.md`) reads it.
- The `scheduled_notifications` Hive box is fronted by a provider so widgets never touch Hive directly.

## What we avoid

- `Provider`-package-style inherited providers for app state. Riverpod replaces them.
- Global singletons not behind a provider — they cannot be overridden in tests.
- Pulling Ferry results into widget-local state; the cache is the source of truth.
