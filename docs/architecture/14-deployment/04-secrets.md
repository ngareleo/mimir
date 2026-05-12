This file covers where each secret lives and which system reads it.

## Server runtime (Fly secrets)

Set via `fly secrets set` for the `mimir-server` app. Read by the Rust binary at startup:

- `NEO4J_URI`, `NEO4J_USER`, `NEO4J_PASSWORD` — AuraDB credentials.
- `JWT_SIGNING_KEY` — symmetric secret for JWT HS256 signing.
- `GOOGLE_OAUTH_CLIENT_ID`, `GOOGLE_OAUTH_CLIENT_SECRET` — for the `/auth/google/callback` exchange.
- `TIGRIS_ACCESS_KEY_ID`, `TIGRIS_SECRET_ACCESS_KEY` — for issuing pre-signed URLs.
- `FCM_SERVICE_ACCOUNT_JSON` — FCM v1 API service account credentials; the full JSON as a single secret value.
- `AXIOM_API_TOKEN`, `AXIOM_DATASET` — OTel exporter target.

## CI / GitHub Actions secrets

- `FLY_API_TOKEN` — `fly deploy`, secret rotation.
- `SWEEP_TOKEN` — scheduled cleanup authentication.

## Client (compile-time)

Public-by-design values baked into the client binary. They are not secrets but should be parametrised via build flavours:

- Sentry DSN.
- Google OAuth client ID.
- FCM project number / sender ID.

## Local dev

`.env` files at `apps/server/.env.local` (gitignored). A template at `apps/server/.env.example` documents every variable. Developers copy the template, fill in their own AuraDB Free credentials, and run.
