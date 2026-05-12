This file covers how to read the known-weaknesses folder and the principles behind it.

## What counts as a weakness

A weakness is a place where Mimir's v1 architecture **does less than the product ultimately needs**, by deliberate choice. Three flavours:

- **Accepted loss:** the failure mode is real but rare and the cost to prevent it is high (e.g., FCM pushes lost on SIGTERM).
- **Deferred work:** a feature exists in the design space but ships in v2 (e.g., GraphQL subscriptions, Sign in with Apple).
- **Constraint:** an external limit we work around but cannot remove (e.g., AuraDB has no Africa region).

It is **not** a weakness if:

- It's an unknown bug — those go in issue tracking.
- It's a stylistic choice (pure GraphQL over REST, etc.) — those live in the topic docs.
- It's a missing v2-and-beyond feature with no current cost — those belong in a product roadmap, not here.

## How to read each file

Each weakness in this folder is presented with three lines:

- **What:** the limitation in one sentence.
- **Why:** the reason it exists (constraint, deliberate cut, accepted risk).
- **Cost / mitigation:** what users or operators feel, and what compensates today.

## When to revisit

A weakness graduates out of this folder when one of:

- The user impact crosses a threshold (e.g., a recurring incident).
- The cost to remove drops (e.g., AuraDB ships an Africa region).
- A v2 milestone explicitly addresses it.

When a weakness is resolved, its file is **deleted**, not edited to say "fixed". This folder always describes the current state of the system, not its history.
