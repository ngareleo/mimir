This file covers the Ferry client and its persisted cache.

## Choice

Ferry is the GraphQL client. No Dio, no other HTTP layer for GraphQL operations. Ferry was chosen because it ships a normalised cache, supports persistence out of the box, has a clean link abstraction for request middleware, and has first-class codegen.

## Cache

The Ferry cache is normalised by GraphQL type and ID and persisted to a Hive box. This is the read source of truth when the device is offline. Eviction is bounded by Hive box size; full strategy lives in `docs/architecture/10-offline-and-sync/01-cache-strategy.md`.

## Link chain

In order, every request passes through:

1. **Auth link** — attaches the current access token from secure storage. On 401, asks the auth provider to rotate via refresh and retries once. See `docs/architecture/08-authentication/03-jwt.md`.
2. **Trace link** — injects a fresh W3C `traceparent` per operation. See `docs/architecture/04-network-and-graphql/04-trace-propagation.md`.
3. **HTTP link** — issues the request to the server's `/graphql` endpoint.

Errors surface to the caller as typed `OperationResponse` values; the auth link is the only one that retries automatically.

## Writes and connectivity

Mutations require connectivity. Ferry does not retry failed mutations on its own, and the client does not queue them. The offline UX rule lives in `docs/architecture/10-offline-and-sync/02-write-policy.md`.

## What does not go through Ferry

The REST exceptions (`docs/architecture/04-network-and-graphql/03-rest-exceptions.md`) and Tigris pre-signed upload PUTs (`docs/architecture/13-file-storage/01-presigned-upload-flow.md`). Everything else is GraphQL.
