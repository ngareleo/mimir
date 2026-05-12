This file covers client-side routing.

## Choice

`go_router` is the routing layer. It is declarative, supports nested shells, and gives the web target real URLs that match the native deep-link structure.

## Why `go_router`

- **Web URL parity is non-negotiable.** The web target needs shareable, refreshable URLs. `go_router` makes routes a first-class declarative tree rather than imperative `Navigator` calls.
- **Deep linking works the same on every platform.** Universal links on iOS, App Links on Android, and direct URLs on web all flow through one route table.
- **Plays nicely with Riverpod.** A `refreshListenable` driven by the `AuthStateProvider` re-evaluates redirects when auth changes — no manual `push`/`pop` choreography on sign-in or sign-out.

## Conventions

- One route table at the app root. Sub-shells (e.g. the bottom-nav shell) use `ShellRoute`.
- Routes are keyed by stable names; widgets navigate by name, never by hard-coded path strings.
- Redirects are pure functions of `AuthState` + the target location. The router never reads providers it does not depend on through `refreshListenable`.
- Deep links into shared resources (timetables, semesters) reuse the same routes as in-app navigation. There is no separate "shared-link" route family.

## Web caveats

- The web target uses path-based URLs (no hash). The Fly server is responsible for serving `index.html` on unknown paths during local dev; in production the web build is hosted separately and a CDN handles the rewrite.
- Browser back/forward must traverse the route stack the same way the native back button does. This is the default `go_router` behaviour; we do not customise it.
