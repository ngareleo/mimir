# Authentication

Mimir authenticates users with Google OAuth and an email/password fallback, issuing JWT access and refresh tokens. Sign in with Apple is deferred until iOS App Review requires it. The auth surface uses the GraphQL API except for the one OAuth callback REST endpoint documented under `docs/architecture/04-network-and-graphql/03-rest-exceptions.md`.

## Direct children

- [00-overview.md](00-overview.md) — the authentication model end-to-end.
- [01-google-oauth.md](01-google-oauth.md) — Google sign-in flow and the callback REST exception.
- [02-email-password.md](02-email-password.md) — registration, sign-in, password reset.
- [03-jwt.md](03-jwt.md) — access and refresh tokens, rotation, revocation.
- [04-apple-deferred.md](04-apple-deferred.md) — when and why to add Sign in with Apple.
