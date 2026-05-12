This file covers the authentication model end-to-end.

## Identity providers

- **Google OAuth** — the primary path. Most users sign in with their Google account.
- **Email/password** — the fallback. Users without a Google account, or who explicitly opt for it, register with email and password.
- **Sign in with Apple** — deferred. Only added if iOS App Review flags App Store guideline 4.8. See `docs/architecture/08-authentication/04-apple-deferred.md`.

A `:User` node may be linked to either or both providers — a Google `sub` claim and a password hash can coexist on the same `:User`.

## Tokens

After a successful sign-in (any provider), the server issues a JWT pair:

- **Access token** — short-lived (15 minutes), HS256, signed with a server secret. Carries `sub` (user ID) and `iat`/`exp`.
- **Refresh token** — long-lived (30 days), opaque random string. Stored server-side, scoped per device.

Rotation rules live in `docs/architecture/08-authentication/03-jwt.md`.

## What the client does

The client stores both tokens in platform secure storage (Keychain on iOS, Keystore on Android, IndexedDB via a wrapped library on web). The Ferry auth link attaches the access token on every request and triggers a refresh on 401.

## What the server does

Every GraphQL operation runs through an auth extractor middleware that resolves the bearer token to a `:User` identity (or anonymous). Resolvers gate themselves; there is no all-or-nothing auth wall.

## REST surface

The only REST endpoints touched by auth are:

- `GET /auth/google/callback` — OAuth code exchange landing.
- `GET /auth/reset?token=…` — password-reset link landing.

Both are in the documented REST exception list.
