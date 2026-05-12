This file covers Mimir's approach to user-uploaded files.

Mimir stores user-uploaded files — avatars in v1, with attachments (assignment submissions, scanned past papers) on the v2 roadmap — in Tigris on Fly. The server never reads or writes the bytes itself. Uploads run through pre-signed URLs returned from GraphQL mutations: the client receives a short-lived URL, PUTs binary data directly to Tigris, then sends a second mutation to mark the upload complete.

This pattern fits two of Mimir's constraints. First, it keeps the API pure GraphQL on the wire — no multipart frames cross the Rust server. Second, it avoids paying server CPU and bandwidth for byte movement that the object store can do directly. The cost is a two-mutation round-trip per upload and the need to handle abandoned `:PendingUpload` records when the client never calls `completeUpload`.

For the flow detail, see [01-presigned-upload-flow.md](01-presigned-upload-flow.md). For the Tigris bucket configuration, see [02-tigris-setup.md](02-tigris-setup.md).
