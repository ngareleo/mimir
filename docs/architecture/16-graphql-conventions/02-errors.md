This file covers the error contract Mimir's GraphQL API surfaces.

## Two error channels

1. **GraphQL-level errors** (parse, validation, schema mismatch) appear in the response's top-level `errors` array. Clients should not see these in production except for the rare bug; they indicate the client and server are out of sync.
2. **Application errors** (invalid input, unauthorized, not-found, conflict) are returned in the same `errors` array, but with a populated `extensions` object the client uses for branching.

## `extensions` shape

```json
{
  "code": "UNAUTHORIZED",
  "category": "auth",
  "traceId": "<32 hex chars>"
}
```

- `code` — stable, machine-readable identifier. Codes are added consciously, not invented per-resolver.
- `category` — one of `auth`, `input`, `permission`, `notFound`, `conflict`, `rateLimit`, `internal`.
- `traceId` — the W3C trace ID for the request. See `docs/architecture/12-telemetry/03-trace-correlation.md`.

## Canonical codes

- `UNAUTHORIZED` — request lacks valid authentication.
- `FORBIDDEN` — authenticated but not allowed (e.g., non-moderator approving a change).
- `NOT_FOUND` — requested node does not exist or is not visible to the caller.
- `INVALID_INPUT` — payload failed validation; `extensions.fieldErrors` may carry per-field detail.
- `CONFLICT` — operation contradicts current state (e.g., approving an already-resolved request).
- `RATE_LIMITED` — too many requests; `extensions.retryAfterMs` is set.
- `INTERNAL` — caught panic or unexpected error; logged with the trace ID; the client should show a generic failure.

## What this is NOT

Mimir does not use union return types for errors. The cost (every resolver returning `OkOrError` types) outweighs the benefit for the codes we surface. If a v2 use case demands typed errors at the schema level, a single resolver can switch without changing the global convention.
