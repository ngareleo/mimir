This file covers why Sign in with Apple is deferred and the conditions for adding it.

## Decision

Sign in with Apple is not in v1. The client ships with Google OAuth and email/password only.

## Why deferred

- The audience is Kenyan university students; Google account coverage is high and Apple device share is lower than in the markets where 4.8 is most aggressively enforced.
- Adding Apple sign-in costs additional client SDK integration, a separate identity-linking flow on the server, and an additional secret in Fly.
- The risk we accept: iOS App Review may flag the missing option under App Store guideline 4.8, which requires apps offering a third-party social sign-in (Google) to also offer Sign in with Apple — *unless* the app uses its own account system (which email/password qualifies as a partial answer to). The interpretation is reviewer-dependent.

## Trigger to add

Sign in with Apple is added if and only if iOS App Review explicitly cites guideline 4.8 against a Mimir submission. We do not pre-emptively ship it.

## What adding it would entail

When the trigger fires:

1. Add the Apple sign-in client SDK to the iOS target (and a stub on Android/web that does nothing — there is no cross-platform Apple SDK).
2. Add a GraphQL mutation `signInWithApple(identityToken: String!)` that verifies Apple's identity token, finds or creates a `:User`, and returns the JWT pair.
3. Add an `appleSubject` property to `:User`, matching the role `googleSubject` plays today.
4. Add an Apple sign-in service ID, key, and Team ID to Fly secrets.

The data model is already prepared: a `:User` can carry multiple linked identity providers without schema changes. The work is integration, not redesign.

## What does not change

- Email/password remains. Apple is an additional option, not a replacement.
- Existing users keep their current sign-in path; linking Apple to an existing account is a settings-screen action, not a forced migration.
