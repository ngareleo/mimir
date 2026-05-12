This file covers the small set of REST endpoints that exist outside the GraphQL surface.

## The exception list

| Method | Path | Purpose |
|---|---|---|
| `GET` | `/auth/google/callback` | OAuth code exchange landing for the web Google sign-in flow. |
| `GET` | `/auth/reset?token=…` | Password-reset link landing — verifies the token and renders or redirects to the reset form. |
| `GET` | `/healthz` | Fly.io liveness probe. Returns 200 when the process can serve traffic. |
| `GET` | `/metrics` | Prometheus/OTel scrape endpoint. |

These four are the *complete* list. The server exposes no other REST endpoints.

## Why these are REST

- **`/auth/google/callback`** — the OAuth provider redirects the browser here with a `code` query parameter. The endpoint exchanges the code for tokens, issues a Mimir JWT pair, and redirects back to the client. GraphQL cannot be the landing target of an external redirect.
- **`/auth/reset`** — the link arrives via email; the browser opens it directly. The endpoint validates the reset token and serves the reset UI (or redirects to it).
- **`/healthz`** — Fly.io probes a plain HTTP endpoint with no body. GraphQL requires a POST and a body.
- **`/metrics`** — Prometheus/OTel collectors scrape a plain `GET`. Not negotiable.

## Rule for adding more

A new REST endpoint requires:

1. A documented reason that GraphQL cannot serve it.
2. An update to this file adding the row.
3. A PR review that explicitly acknowledges the exception.

The list is short on purpose. Most "feels like it should be REST" intuitions resolve to a GraphQL mutation once examined.
