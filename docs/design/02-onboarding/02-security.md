# Security

This file covers the third and final screen of the registration flow.

## Layout

- Screen title: **Security**.
- A vertical stack of two labelled password fields, in order:
  1. **Enter password**
  2. **Confirm password**
- Each field carries a trailing show/hide affordance.
- A primary action button: **Next**, with a forward-arrow affordance.
- Below the button, a secondary link: **I already have an account**, which exits the flow into sign-in.

## Behaviour

Both fields are required. The two entries must match before submit is accepted; mismatch and policy-violation errors render inline beneath the offending field. Password policy (length, character classes) is owned by the auth layer and documented in `docs/architecture/08-authentication/02-email-password.md`.

On **Next**, the full registration payload — identity, phone, and password — is submitted to the server. A successful response yields an authenticated session and routes the user to the Home tab; a server-side error returns control to this screen with an inline message.

## Navigation

- **Entry:** from the phone-verification screen via **Next**.
- **Forward:** **Next** → on success, the Home tab (`docs/design/03-home/index.md`).
- **Exit:** **I already have an account** → sign-in screen.
- **Back:** the platform back gesture returns to phone verification.

## Cross-references

- `docs/architecture/08-authentication/03-jwt.md` for how the resulting session is represented.
