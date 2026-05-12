This file covers how Mimir paginates GraphQL list fields.

## Relay cursor connections

Every list field with more than a handful of expected results returns a Connection:

```graphql
type SemesterConnection {
  edges: [SemesterEdge!]!
  nodes: [Semester!]!
  pageInfo: PageInfo!
  totalCount: Int
}

type SemesterEdge {
  cursor: String!
  node: Semester!
}

type PageInfo {
  hasNextPage: Boolean!
  hasPreviousPage: Boolean!
  startCursor: String
  endCursor: String
}
```

`async-graphql` provides the `connection::Connection` type that produces this shape; resolvers return it with the appropriate node-type parameter.

## Arguments

Standard four-argument pattern: `first`, `after`, `last`, `before`. `first` + `after` is the common forward-paging case; `last` + `before` enables backward paging from a known cursor.

The server enforces a max page size of 50 per request. Requesting more returns an `INVALID_INPUT` error.

## Cursors

Cursors are opaque to clients. The server encodes them as base64 of `{ field, value }` (e.g., `{"createdAt": "2025-12-01T00:00:00Z"}`). Decoding is server-side only; clients pass cursors through verbatim.

## When NOT to paginate

Bounded relationships with predictable small cardinality (e.g., a semester's `moderators`, an event's `links`) return plain `[T!]!` lists. Pagination overhead is unjustified when the upper bound is small and known.

## Client side

Ferry's normalized cache merges Connection results by `endCursor`. Successive `loadMore` calls update the same observable list. See `docs/architecture/04-network-and-graphql/01-ferry-client.md`.
