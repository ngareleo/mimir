# File storage

User-uploaded files (assignment submissions, past papers) live in Tigris on Fly. Uploads go through pre-signed URLs returned from GraphQL mutations; the client PUTs the bytes directly to Tigris and reports completion through a second mutation. The server never touches the bytes.

## Direct children

- [00-overview.md](00-overview.md) — the upload model at a glance.
- [01-presigned-upload-flow.md](01-presigned-upload-flow.md) — the `requestUpload` / PUT / `completeUpload` flow.
- [02-tigris-setup.md](02-tigris-setup.md) — bucket, credentials, lifecycle.
