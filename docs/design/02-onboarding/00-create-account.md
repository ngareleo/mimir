# Create account

This file covers the first screen of the registration flow.

## Layout

- Screen title: **Create account**.
- A vertical stack of four labelled text fields, in order:
  1. **First name**
  2. **Last name**
  3. **Username**
  4. **Email address**
- A primary action button: **Next**, with a forward-arrow affordance.
- Below the button, a secondary link: **I already have an account**, which exits the registration flow into sign-in.

## Behaviour

The four fields are all required. Username uniqueness and email format are validated on submit; validation errors render inline beneath the offending field. **Next** advances to phone verification (see `01-phone-verification.md`) carrying the entered values into the flow's local state. The values are not persisted to the server until the final step succeeds.

## Navigation

- **Entry:** launched by the "Sign up" entry point on the unauthenticated landing screen.
- **Forward:** **Next** → phone verification.
- **Exit:** **I already have an account** → sign-in screen.
- **Back:** there is no app bar on this screen; the platform back gesture returns to the landing screen.

## Cross-references

- `docs/architecture/08-authentication/02-email-password.md` for what happens server-side when the flow completes.
- `docs/product/05-domain-model/01-user.md` for the underlying user entity these fields populate.
