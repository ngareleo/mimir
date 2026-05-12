This file covers the offline contract for the client.

## Contract

- **Reads work offline.** The Ferry persisted Hive cache serves the most recent successful response for each query. The UI renders against the cache and indicates staleness where it matters.
- **Writes require connectivity.** Mutations are not queued. If the device is offline, the mutation surface shows a clear offline state and the action does not run.
- **Notifications keep firing.** The `scheduled_notifications` Hive box is independent of connectivity. Local notifications fire at their scheduled times regardless of network state.

## Why this contract is strict

An offline write queue is technically possible but operationally expensive: mutations would have to be replayed in order, conflict resolution would need to be designed per resource type, and partial-failure visibility (which write succeeded, which is still pending) would need a UI vocabulary the product does not yet have. We decline the complexity in v1.

The cost is one well-named UI state ("you are offline; this action will be available again when you are online") in exchange for a simpler mental model: writes happen when you press the button, every time.

## Mental model

The client is a *cache with a UI*, not a *replica with an outbox*. The server is authoritative; the cache is a read-through that survives going offline.

## What you can do offline

- Open the app, see the home screen, see your timetable, see your tasks and events as they were at last sync.
- Receive any local notification that was already scheduled before going offline.
- Tap a notification and open the relevant screen.

## What you cannot do offline

- Create, edit, or delete any resource.
- Sign in (the auth flow requires the server).
- Upload a file (requires a fresh pre-signed URL).

The UI surfaces this through a single "offline" affordance on action buttons, not a global modal.
