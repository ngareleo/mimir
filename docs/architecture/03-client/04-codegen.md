This file covers client-side code generation.

## What is generated

- **Ferry operations.** `.graphql` files alongside Dart code are compiled into typed request/response classes by Ferry's codegen.
- **Riverpod providers.** `@riverpod`-annotated functions and classes generate the provider boilerplate.
- **Freezed types.** Model classes that need value equality, copy-with, and union variants use `freezed`.
- **JSON serialisers.** Where freezed types cross persistence boundaries (e.g. the `scheduled_notifications` Hive box), `json_serializable` produces `toJson`/`fromJson`.

## How it runs

A single `build_runner` invocation generates everything. The output is committed so CI does not need to run codegen to compile the app; the SDL-drift check in CI (see `docs/architecture/04-network-and-graphql/02-schema-source-of-truth.md`) implicitly covers Ferry-output drift by failing if `.graphql` files reference schema that no longer exists.

## Inputs and outputs

- **Input SDL**: `packages/graphql-schema/schema.graphql`. The client reads it directly; it does not maintain a local copy.
- **Input fragments**: co-located `*.graphql` files inside `apps/client`.
- **Output**: `*.data.gql.dart`, `*.var.gql.dart`, `*.req.gql.dart` next to each `.graphql` file; freezed `*.freezed.dart` and `*.g.dart` next to model files.

## Why this matters here

The SDL is the cross-stack contract. Ferry codegen is the mechanism that makes the contract typed at the call site. Misalignment between client and server cannot survive CI: a server change that drops a field will fail the client's codegen step, which CI runs as part of the test job.
