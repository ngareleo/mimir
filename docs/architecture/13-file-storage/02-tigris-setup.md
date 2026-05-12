This file covers Tigris bucket configuration for Mimir.

## Buckets

Two buckets in production:

- `mimir-prod-uploads` — user-owned files (avatars, future attachments). Read access via per-object pre-signed URLs only.
- `mimir-prod-public` — system-owned files (default avatars, illustration assets). Read access public; written by deploys.

A single bucket `mimir-dev-uploads` covers both roles in development.

## Credentials

The server reads two secrets, both provisioned via Fly:

- `TIGRIS_ACCESS_KEY_ID`
- `TIGRIS_SECRET_ACCESS_KEY`

See `docs/architecture/14-deployment/04-secrets.md`.

## Region

Tigris is globally distributed; objects are served from the closest edge. The server connects via the S3-compatible endpoint at `https://fly.storage.tigris.dev`. No region pinning needed.

## Lifecycle

- **Pending uploads with no `completeUpload` after one hour:** server-side sweep deletes the Tigris object (the bucket itself has no lifecycle rule for this — the sweep job handles it deterministically). See `docs/architecture/13-file-storage/01-presigned-upload-flow.md`.
- **Orphaned objects** (Tigris objects without a referencing `:Asset` node) **older than seven days:** weekly reconciliation job deletes them.

## CORS

PUT is allowed from the client app origins: the Fly app domain (for the web Flutter build) and any additional approved Flutter web origins. The mobile clients PUT from native code with no preflight, so CORS is not their concern.
