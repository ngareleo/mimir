This file covers the Google OAuth flow.

## Flow

### Mobile (Android, iOS)

1. Client launches the Google sign-in UI through the platform SDK.
2. SDK returns an authorisation code via a custom URL scheme handled in-app.
3. Client posts the code to a GraphQL mutation `signInWithGoogle(code: String!)`.
4. Server exchanges the code with Google, verifies the ID token, finds or creates the `:User`, and returns the JWT pair.

### Web

1. Client redirects the browser to Google's authorisation URL with the Mimir callback as `redirect_uri`.
2. Google redirects back to `GET /auth/google/callback?code=…` on the server.
3. The callback endpoint exchanges the code, finds or creates the `:User`, issues the JWT pair, and redirects the browser to a client URL with the tokens (delivered via `postMessage` to the opener window or via a short-lived signed fragment).
4. The client picks up the tokens and stores them.

## The REST exception

`GET /auth/google/callback` is one of the four documented REST endpoints — see `docs/architecture/04-network-and-graphql/03-rest-exceptions.md`. It exists because the OAuth provider must redirect a browser to a plain HTTP endpoint; GraphQL cannot serve as a redirect target. This is the *only* REST exception in the auth subsystem.

## User linking

- First Google sign-in for an unknown email creates a `:User` node with `googleSubject` set and `passwordHash` null.
- Subsequent sign-ins look up the user by `googleSubject` (preferred) or `email` (fallback for users who pre-registered with email/password and later linked Google).
- A user can have both `googleSubject` and `passwordHash` set. Either path completes sign-in.

## What the server stores

- `googleSubject` — the `sub` claim. Stable across email changes.
- `email` — for display and email/password fallback.
- `name` — initial value from the Google profile; user-editable afterwards.

Access and refresh tokens issued from this flow follow the same rotation rules as any other sign-in; see `docs/architecture/08-authentication/03-jwt.md`.
