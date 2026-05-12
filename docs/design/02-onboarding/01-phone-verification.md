# Phone verification

This file covers the second screen of the registration flow.

## Layout

- Screen title: **Phone verification**.
- A single labelled text field: **Phone number**, accepting an international format (the mockup placeholder shows a Kenyan country-code prefix).
- A primary action button: **Next**, with a forward-arrow affordance.
- Below the button, a secondary link: **I already have an account**, which exits the registration flow into sign-in.

## Behaviour

The phone field is required and validated for international-format conformance before submit is accepted. On **Next**, the entered number is held in the flow's local state alongside the identity fields from the previous step; verification challenge handling (one-time code, retry policy, fallback) is owned by the auth layer and described in `docs/architecture/08-authentication/02-email-password.md`.

This screen does not itself prompt for an SMS code in the mockup set — the code-entry sub-step, if needed, is part of the auth contract rather than the structural design captured here.

## Navigation

- **Entry:** from the create-account screen via **Next**.
- **Forward:** **Next** → security screen.
- **Exit:** **I already have an account** → sign-in screen.
- **Back:** the platform back gesture returns to the create-account screen with previously entered values preserved.

## Cross-references

- `docs/product/05-domain-model/01-user.md` for where the phone number is stored on the user entity.
