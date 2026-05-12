# Design

This domain describes the **screen structure** of Mimir — what appears on each screen, how the screens connect, and the navigation primitives that hold them together. It deliberately omits visual treatment: colors, typography, iconography, and motion are out of scope here.

The structural references in `docs/design/assets/` are extracted from a 2023 university-project mockup set. They are reproduced so an engineer or agent can see the intended layout at a glance. The visual design will be redone in full before launch — treat the mockups as wireframes, not as the target look.

## Children

- [00-overview.md](00-overview.md) — purpose of this domain and the structural-only rule.
- [01-information-architecture.md](01-information-architecture.md) — the app's global navigation: bottom tabs, app bar, FAB.
- [02-onboarding/](02-onboarding/index.md) — registration flow across three screens.
- [03-home/](03-home/index.md) — the Home tab in empty and populated states, plus its FAB menu.
- [04-timetable/](04-timetable/index.md) — creating, viewing, and managing a timetable.
- [05-semester/](05-semester/index.md) — creating a semester, its tabbed content, and the approvals queue.
- [06-forms/](06-forms/index.md) — shared modal forms for tasks, events, filters, and timetable links.

See also `docs/product/03-features.md` for the feature inventory these screens implement.
