This file covers the three-step pre-signed upload sequence.

## The mutations

1. `requestUpload(input: { kind, contentType, sizeBytes })` returns `{ uploadUrl, key, expiresAt }`. The server validates kind/contentType/size against per-kind policy, records a `:PendingUpload { key, owner, kind, createdAt }` node, and asks Tigris for a 15-minute pre-signed PUT URL.
2. The client PUTs the bytes directly to `uploadUrl`. The PUT is not authenticated against Mimir; Tigris validates the signed URL.
3. `completeUpload(key)` checks that the object exists in Tigris, links the `:PendingUpload` to the owning entity (e.g., a `:User { id }`), and returns the canonical asset reference. The mutation is idempotent — duplicate calls with the same key resolve to the same asset.

## Failure modes

- **Client never calls `completeUpload`.** A scheduled job (see `docs/architecture/14-deployment/03-ci-cd.md`) sweeps `:PendingUpload` nodes older than one hour and deletes the corresponding Tigris objects.
- **Tigris reports the object missing on `completeUpload`.** The mutation returns an error with `extensions.code = "UPLOAD_NOT_FOUND"` and leaves the `:PendingUpload` node in place for the sweep to retry.
- **Size mismatch.** Tigris does not enforce `sizeBytes`. The server checks `Content-Length` from a HEAD request on `completeUpload` and rejects mismatches.

## Trace correlation

The pre-signed PUT itself is a direct client→Tigris call and is not part of any Mimir-side trace. Both mutations carry the usual `traceparent` header (see `docs/architecture/12-telemetry/03-trace-correlation.md`); the PUT is invisible between them.
