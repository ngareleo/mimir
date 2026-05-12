# Mimir docs

Mimir is a student-companion app for Kenyan university students — a rebuild of a 2023 Kenyatta University final-year project. These docs ground future engineers and AI agents on **what** we're building, **how** it should be structured visually, and **how** it's architected technically.

The docs split into three domains. Each folder has its own `index.md` with one-sentence descriptions of its children. Drill in from this root; don't re-read prose at the higher levels once you've descended.

## Map

- [`product/`](product/index.md) — what Mimir is and who it's for
- [`design/`](design/index.md) — screen structure (visual design TBD)
- [`architecture/`](architecture/index.md) — technical decisions and contracts

## Full tree

```
docs/
├── index.md
│
├── product/
│   ├── index.md
│   ├── 00-overview.md
│   ├── 01-problem.md
│   ├── 02-users.md
│   ├── 03-features.md
│   ├── 04-use-cases/
│   │   ├── index.md
│   │   ├── 00-overview.md
│   │   ├── 01-creating-resources.md
│   │   ├── 02-sharing-resources.md
│   │   ├── 03-approving-changes.md
│   │   └── 04-moderation.md
│   ├── 05-domain-model/
│   │   ├── index.md
│   │   ├── 00-overview.md
│   │   ├── 01-user.md
│   │   ├── 02-semester.md
│   │   ├── 03-unit.md
│   │   ├── 04-lecture.md
│   │   ├── 05-timetable.md
│   │   ├── 06-task.md
│   │   ├── 07-event.md
│   │   ├── 08-assignment.md
│   │   ├── 09-paper.md
│   │   └── 10-shared-ownership.md
│   ├── 06-notifications/
│   │   ├── index.md
│   │   ├── 00-overview.md
│   │   ├── 01-time-based.md
│   │   └── 02-event-based.md
│   └── 07-scope.md
│
├── design/
│   ├── index.md
│   ├── 00-overview.md
│   ├── 01-information-architecture.md
│   ├── 02-onboarding/
│   │   ├── index.md
│   │   ├── 00-create-account.md
│   │   ├── 01-phone-verification.md
│   │   └── 02-security.md
│   ├── 03-home/
│   │   ├── index.md
│   │   ├── 00-empty-state.md
│   │   ├── 01-populated.md
│   │   └── 02-fab-menu.md
│   ├── 04-timetable/
│   │   ├── index.md
│   │   ├── 00-create.md
│   │   ├── 01-view.md
│   │   └── 02-share-and-manage.md
│   ├── 05-semester/
│   │   ├── index.md
│   │   ├── 00-create.md
│   │   ├── 01-tabs.md
│   │   └── 02-approvals.md
│   ├── 06-forms/
│   │   ├── index.md
│   │   ├── 00-task-form.md
│   │   ├── 01-event-form.md
│   │   ├── 02-filters.md
│   │   └── 03-link-to-timetable.md
│   └── assets/
│       ├── registration.png
│       ├── timetable.png
│       ├── semester.png
│       ├── home.png
│       └── forms.png
│
└── architecture/
    ├── index.md
    ├── 00-overview.md
    ├── 01-stack.md
    ├── 02-repo-structure.md
    ├── 03-client/
    │   ├── index.md
    │   ├── 00-overview.md
    │   ├── 01-state-management.md
    │   ├── 02-routing.md
    │   ├── 03-platforms.md
    │   └── 04-codegen.md
    ├── 04-network-and-graphql/
    │   ├── index.md
    │   ├── 00-overview.md
    │   ├── 01-ferry-client.md
    │   ├── 02-schema-source-of-truth.md
    │   ├── 03-rest-exceptions.md
    │   └── 04-trace-propagation.md
    ├── 05-server/
    │   ├── index.md
    │   ├── 00-overview.md
    │   ├── 01-module-layout.md
    │   ├── 02-cargo-bins.md
    │   └── 03-error-handling.md
    ├── 06-database/
    │   ├── index.md
    │   ├── 00-overview.md
    │   ├── 01-tiers-and-region.md
    │   ├── 02-migrations.md
    │   └── 03-constraints-and-indexes.md
    ├── 07-data-model/
    │   ├── index.md
    │   ├── 00-overview.md
    │   ├── 01-naming-and-ids.md
    │   ├── 02-user.md
    │   ├── 03-semester.md
    │   ├── 04-unit-lecture.md
    │   ├── 05-timetable.md
    │   ├── 06-task-event.md
    │   ├── 07-assignment-paper.md
    │   └── 08-sharing.md
    ├── 08-authentication/
    │   ├── index.md
    │   ├── 00-overview.md
    │   ├── 01-google-oauth.md
    │   ├── 02-email-password.md
    │   ├── 03-jwt.md
    │   └── 04-apple-deferred.md
    ├── 09-notifications/
    │   ├── index.md
    │   ├── 00-overview.md
    │   ├── 01-local-scheduling.md
    │   ├── 02-fcm-push.md
    │   └── 03-push-to-cache-bridge.md
    ├── 10-offline-and-sync/
    │   ├── index.md
    │   ├── 00-overview.md
    │   ├── 01-cache-strategy.md
    │   ├── 02-write-policy.md
    │   └── 03-reconciliation.md
    ├── 11-sharing-and-permissions/
    │   ├── index.md
    │   ├── 00-overview.md
    │   ├── 01-ownership.md
    │   ├── 02-moderators.md
    │   ├── 03-approval-flow.md
    │   └── 04-fan-out.md
    ├── 12-telemetry/
    │   ├── index.md
    │   ├── 00-overview.md
    │   ├── 01-server-otel.md
    │   ├── 02-client-sentry.md
    │   └── 03-trace-correlation.md
    ├── 13-file-storage/
    │   ├── index.md
    │   ├── 00-overview.md
    │   ├── 01-presigned-upload-flow.md
    │   └── 02-tigris-setup.md
    ├── 14-deployment/
    │   ├── index.md
    │   ├── 00-overview.md
    │   ├── 01-fly-server.md
    │   ├── 02-auradb.md
    │   ├── 03-ci-cd.md
    │   └── 04-secrets.md
    ├── 15-testing/
    │   ├── index.md
    │   ├── 00-overview.md
    │   ├── 01-client-widget-and-golden.md
    │   └── 02-server-cargo-and-neo4j.md
    └── 16-graphql-conventions/
        ├── index.md
        ├── 00-overview.md
        ├── 01-pagination.md
        ├── 02-errors.md
        ├── 03-naming.md
        └── 04-ids.md
```
