# Overview

This file covers the purpose and scope of the design domain.

**The mockups in `docs/design/assets/` are structural references only.** They were produced for a 2023 final-year project and depict an early version of the app. The visual design — colors, typography, iconography, spacing, motion, and any decorative treatment — **will be entirely redone** before Mimir ships. Nothing about the look of those screens should be taken as a target.

What these documents pin down is the **structure**: which screens exist, what each screen contains, what controls live where, and how a user moves between them. That structure is stable across the rebuild because it reflects the product's information model, not its surface.

When reading any leaf document under this domain, expect:

- A list of screen regions and what they hold.
- Form fields and their order.
- Navigation entry points and exits.
- Cross-references to the underlying product features and domain entities.

When reading any leaf document, do **not** expect:

- Color palettes, gradients, or theme tokens.
- Font families, type scales, or weights.
- Pixel-exact dimensions, breakpoints, or spacing values.
- Motion specifications, transitions, or animation curves.

Those decisions live with the future visual-design pass and are intentionally absent here. If a future doc adds visual specification, it belongs in a new design-system tree, not in this structural reference.

See `docs/product/03-features.md` for what the screens are for, and `docs/architecture/03-client/03-platforms.md` for platform-parity caveats that affect what some screens can do on the web.
