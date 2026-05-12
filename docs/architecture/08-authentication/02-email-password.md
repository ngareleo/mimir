This file covers the email/password authentication path.

## Registration

A GraphQL mutation `registerWithEmail(email: String!, password: String!, name: String!)`:

1. Validates the email shape and password strength.
2. Looks up an existing `:User` by email. If present and `passwordHash` is null (Google-only account), links by setting `passwordHash`. If present with a password, returns a `CONFLICT` error.
3. Otherwise creates a `:User` node with `passwordHash` (Argon2id).
4. Returns the JWT pair.

There is no separate "verify your email" gate in v1. The product accepts this risk; mitigation is rate-limiting and reset-token signing.

## Sign-in

A GraphQL mutation `signInWithEmail(email: String!, password: String!)`:

1. Looks up the user by case-folded email.
2. Verifies the password against `passwordHash` using Argon2id.
3. Returns the JWT pair on success; `UNAUTHENTICATED` otherwise.

The error response shape does not distinguish between "unknown email" and "wrong password".

## Password reset

The flow combines a GraphQL mutation, an email, and the second REST exception:

1. Client calls `requestPasswordReset(email: String!)`. Server issues a signed reset token (15-minute TTL) and emails a link of the form `https://<server>/auth/reset?token=…`.
2. The user clicks the link. The server's `GET /auth/reset` endpoint validates the token and serves (or redirects to) the reset UI.
3. The reset UI calls `completePasswordReset(token: String!, newPassword: String!)` — a GraphQL mutation. Server validates the token, updates `passwordHash`, and invalidates the token.

Reset tokens are signed, opaque, and single-use. They are not stored as nodes; the signature carries enough to reconstruct the user binding.

## Storage

`passwordHash` is Argon2id, parameters tuned for ~250ms on the production machine class. The plaintext password never leaves the server's request handler.
