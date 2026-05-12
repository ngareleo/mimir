This file covers the Flutter test approach on the client.

## Widget tests

Each screen has a widget test that:

1. Constructs the widget under test with `pumpWidget`.
2. Wraps it in `MaterialApp` plus a `ProviderScope` overriding the relevant Riverpod providers with stub values.
3. Replaces the Ferry client with a `StubClient` whose responses come from a per-test fixture map keyed by operation name plus variables.
4. Asserts on visible widgets via `find.byType` / `find.text` and on user interactions via `tester.tap` / `tester.enterText`.

Files live alongside the widget under test (e.g., `screens/home/home_screen_test.dart` next to `home_screen.dart`).

## Golden tests

Golden coverage targets the **structural mockups** rather than every screen state. Files committed under `apps/client/test/goldens/`:

- `home_empty.png`, `home_populated.png`
- `semester_tabs.png`, `semester_approvals.png`
- `timetable_view.png`, `timetable_create.png`
- `forms_task.png`, `forms_event.png`

Tests use `matchesGoldenFile`. Regeneration is an explicit step (`flutter test --update-goldens`) gated by code review confirming the regenerated images correspond to intentional changes.

## Hooks tests

Custom hooks (e.g., `useScheduledNotifications`) are tested in isolation by mounting them inside a `HookBuilder` test widget and asserting on the produced state via `tester.binding`.

## What is not covered

- Real Ferry client behaviour (operation execution, caching) — not exercised in widget tests; covered only in server integration tests against the GraphQL endpoint.
- Platform channels (notifications, FCM) — mocked at the channel level using `TestDefaultBinaryMessenger`.
