This file covers the JWT access and refresh token model.

## Access token

- **Format**: JWT, HS256, signed with a server secret loaded from Fly secrets.
- **Lifetime**: 15 minutes.
- **Claims**: `sub` (user ID), `iat`, `exp`. Nothing else; we do not pack roles or denormalised state into the token.
- **Transport**: `Authorization: Bearer <token>` on every GraphQL request (and on any client→server REST call requiring auth).

The access token is verifiable without a database call. This is the only reason we use JWTs for the access layer.

## Refresh token

- **Format**: opaque high-entropy random string (256 bits, base64url).
- **Lifetime**: 30 days, sliding (rotated on every refresh).
- **Storage**: server-side as a `:RefreshToken` node bound to a user and a device fingerprint. Client stores in platform secure storage.
- **Scope**: per device. Signing out on one device does not invalidate other devices' refresh tokens.

## Rotation on refresh

A GraphQL mutation `refreshSession(refreshToken: String!)`:

1. Looks up the refresh token node. If absent or revoked, returns `UNAUTHENTICATED`.
2. Issues a *new* access token and a *new* refresh token.
3. Marks the old refresh token as revoked.
4. Returns both new tokens.

This is rotation: every refresh changes the refresh token. Reuse of an already-rotated refresh token is treated as compromise — all of the user's refresh tokens are revoked. The client signs the user out and forces re-authentication.

## Revocation

- **Sign-out** revokes the current device's refresh token only.
- **Sign-out everywhere** revokes every refresh token bound to the user.
- **Compromise detection** (reused rotated token) revokes every refresh token bound to the user.

Access tokens are not revocable in v1; the 15-minute window is the bound.

## Server boundary

Both endpoints — sign-in mutations and `refreshSession` — return the same `AuthResult { accessToken, refreshToken, user }` shape. Client code paths converge after this point.
