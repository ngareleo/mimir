This file covers Mimir's ID strategy.

## Opaque global IDs

Every node-like type exposes a single `id: ID!` field. IDs are opaque strings — clients treat them as bytes and pass them back unchanged. Decoding the structure is a server-side concern.

The encoding is `base64url(<type-tag>:<uuidv7>)`, where:

- `type-tag` is the GraphQL type name (`Semester`, `User`, ...).
- `uuidv7` is the underlying time-ordered UUID stored on the node in Neo4j as the `id` property.

UUIDv7 is chosen because it sorts by creation time, which gives free ordering for cursor pagination and makes index lookups cache-friendly. UUIDv4 would lose the ordering benefit; integer auto-increments would leak cardinality.

## Resolution

A top-level `node(id: ID!): Node` query (Relay-style) decodes the ID, dispatches to the per-type resolver, and returns the node — or `null` if it doesn't exist or isn't visible.

`Node` is a GraphQL interface implemented by every type that exposes `id`. Every persistent node type implements it; transient leaf objects (e.g., `PageInfo`) do not.

## Why opaque

Opaque IDs let the server change the underlying storage (split a type, change UUID version, migrate to a different key) without breaking clients. The client guarantees it has never made assumptions about the bytes.

## ID equality

Two IDs are equal as strings if and only if they refer to the same node. Comparing IDs is a string comparison; no parsing required.
