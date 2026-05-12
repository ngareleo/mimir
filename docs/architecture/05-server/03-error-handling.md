This file covers the error contract surfaced through `async-graphql`.

## Two categories of failure

- **User-facing errors** carry information the client should act on (validation failed, not found, unauthorised, forbidden, conflict). They appear in the GraphQL `errors` array with a stable `code` extension.
- **Internal errors** (database unavailable, panics caught at the executor, downstream HTTP failures) appear in the `errors` array with code `INTERNAL`. They never leak server internals; the human-readable message is generic.

## Shape

Every error includes the following extensions:

```graphql
extensions: {
  code: "NOT_FOUND" | "UNAUTHENTICATED" | "FORBIDDEN" | "VALIDATION" | "CONFLICT" | "INTERNAL",
  traceId: "<32-hex W3C trace id>"
}
```

`code` is the only contract the client matches on. `traceId` echoes the incoming `traceparent`'s trace ID so the client can tag Sentry events for cross-stack correlation; detail in `docs/architecture/12-telemetry/03-trace-correlation.md`. Field-level error rules live in `docs/architecture/16-graphql-conventions/02-errors.md`.

## How resolvers signal failure

Resolvers return `Result<T, AppError>`. `AppError` is a single enum with one variant per code. A `From` impl converts it into `async_graphql::Error` with the right extensions. Resolvers never construct `async_graphql::Error` directly.

`AppError::Internal` wraps the underlying error for the OTel span; the human-facing message stays generic. Anything not classified is treated as `Internal`.

## HTTP-level failures

The REST exception list returns standard HTTP status codes — `400` for invalid input, `401`/`403` for auth, `404` for unknown tokens, `500` for unexpected. They do not carry the GraphQL error shape; they are HTML or redirects.

## What we do not do

- We do not return `null` for failed fields with the error on the side. Resolver failures bubble up; partial data is not designed-for in v1.
- We do not use HTTP status codes from `/graphql` to signal user errors; `/graphql` returns `200` with errors in the body, per GraphQL convention.
