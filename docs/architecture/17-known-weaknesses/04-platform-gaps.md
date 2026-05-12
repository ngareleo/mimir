This file covers per-platform feature degradation.

## iOS 64-pending local notification cap

- **What:** iOS allows at most 64 scheduled local notifications per app.
- **Why:** Apple platform limit.
- **Cost / mitigation:** the rolling-window scheduler keeps only the next ~7 days of items scheduled and refills on app foreground and after each FCM data message. A user with very many simultaneously-relevant deadlines may briefly see fewer than expected reminders if more than 64 items fall in the window — rare in practice. See `docs/architecture/09-notifications/01-local-scheduling.md`.

## Web has no local notifications

- **What:** Flutter Web cannot schedule OS-level alarms. Local notifications (types 1–5) do not fire on web.
- **Why:** the Web Notifications API has no scheduled-alarm primitive. Service workers can fire in response to push but cannot pre-schedule.
- **Cost / mitigation:** web users receive only FCM push for types 6–8 plus opportunistic in-page notifications when the tab is open. Documented in `docs/architecture/03-client/03-platforms.md`.

## No Sign in with Apple

- **What:** OAuth is Google-only in v1; email/password is the non-Google fallback.
- **Why:** deferred to keep the Apple Developer setup cost out of v1. The email/password fallback may satisfy App Store guideline 4.8.
- **Cost / mitigation:** if iOS App Review rejects under 4.8, we add Sign in with Apple as a fast follow. The auth doc reserves the seat. See `docs/architecture/08-authentication/04-apple-deferred.md`.

## Web push spotty on older browsers

- **What:** FCM web push relies on the Push API. Safari supports it only from 2023 (iOS 16.4+, macOS 13+). Some older browsers don't get pushes at all.
- **Why:** browser-vendor limitation.
- **Cost / mitigation:** users on unsupported browsers see updated state on tab refresh or app foreground; nothing else degrades.
