# Known weaknesses

This folder catalogues the v1 architecture's known weaknesses — limitations, accepted risks, and deliberate cuts. Each is a tradeoff made consciously to ship v1 with a small team. They're documented here so engineers and AI agents can recognise them, plan around them, and revisit them when the product earns the right to.

The weaknesses are organised by their dominant cost dimension. Severity is informal: most are minor irritants in v1; a few are tail risks that could bite if the product grows faster than expected.

## Direct children

- [00-overview.md](00-overview.md) — how to read this folder; what counts as a weakness; what's deferred for v2.
- [01-availability.md](01-availability.md) — single points of failure and backup gaps.
- [02-latency.md](02-latency.md) — regional latency tradeoffs.
- [03-reliability.md](03-reliability.md) — accepted-loss patterns and missing test coverage.
- [04-platform-gaps.md](04-platform-gaps.md) — per-platform feature degradation.
- [05-operational.md](05-operational.md) — operational debt: tooling, process, infrastructure.
