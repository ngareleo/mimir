# Create

This file covers the screen used to create a new semester.

## Layout

- Global app bar with a back arrow and the title **Semester**.
- A large free-text field used as the semester label, with placeholder text **Semester label...**.
- A row of two date pickers, side by side: **Pick start date** and **Pick end date**.
- A primary action button: **Create semester**.
- The three-tab bottom navigation persists at the foot of the screen.

The screen has no FAB.

## Behaviour

The label is required. Both dates are optional but, when supplied, the end date must not precede the start date; a violation surfaces inline. On **Create semester**, the form submits and the user is routed to the semester detail view (`01-tabs.md`) on its **All** tab, which renders empty for a newly created semester.

This screen is reachable from the Home FAB's **Create semester** option (`docs/design/03-home/02-fab-menu.md`).

## Cross-references

- `docs/product/05-domain-model/02-semester.md` for the underlying entity and its fields.
- `docs/product/05-domain-model/03-unit.md` for how units attach to a semester after creation.
