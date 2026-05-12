This file covers what is in and out of scope for Mimir v1.

**In scope.**

- Students enrolled at universities **in Kenya**. The product assumes a Kenyan semester structure (two long semesters with units, CATs, and end-of-semester papers) and Kenyan administrative deadlines (unit registration, fee payment).
- Web, iOS, and Android. The Flutter client targets all three; web parity is full for reads and writes but has documented gaps for local notification scheduling (see `docs/architecture/03-client/03-platforms.md`).
- Personal reminders and notifications that **work offline**. Time-based notifications (types 1–5) are scheduled on the device and fire without connectivity.
- Sharing of timetables and semesters with other Mimir users, with an approval flow for changes by non-owners.

**Out of scope.**

- **International universities.** The data model and UI are tuned to the Kenyan format; supporting other countries is deferred.
- **High-school or primary-school students.** Most Kenyan high-school students do not carry smartphones; the audience is university only.
- **In-app chat, networking, or social features.** Mimir is not a communication platform. Students still talk on WhatsApp; Mimir replaces the information-store role only.
- **Offline writes.** Reads use the persisted GraphQL cache and work without connectivity. **Writes (creating, editing, sharing, approving) require an internet connection.** The client surfaces a clear offline state when a mutation cannot run. This is a deliberate v1 simplification — see `docs/architecture/10-offline-and-sync/02-write-policy.md`.
- **Sign in with Apple** unless required by App Review. See `docs/architecture/08-authentication/04-apple-deferred.md`.

The wider product motivation lives in `docs/product/01-problem.md`.
