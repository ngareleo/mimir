This file covers what Mimir is and the shape of the product at a glance.

Mimir is a student-companion app for Kenyan university students. It centralises the information a student needs to keep track of school — lectures, assignments, continuous assessment tests (CATs), exams, and school deadlines such as unit registration and fee payment — and surfaces it through reminders and notifications.

The app is opinionated for Kenyan university workflows. A student creates a **semester** that bundles their units, a **timetable** of lectures, and academic items (**assignments**, **CATs**, **exams**, **tasks**, **events**) that live inside that semester. The semester is the unit of sharing: a course representative can share their semester with classmates so an assignment posted once reaches everyone.

Shared resources have an **owner** (the original creator, who becomes an Admin on sharing) and zero or more **moderators**. When a non-owner edits a shared resource, the change enters an approval queue; an admin or moderator approves or rejects it. This keeps shared information trustworthy without funnelling all edits through a single person.

Notifications fall into two transport classes. Personal time-based reminders (types 1–5: lecture, assignment deadline, CAT, exam, school deadline) fire locally on the device and work offline. Cross-user event-based pushes (types 6–8: shared-resource change, resource shared with you, approval-pending) arrive via FCM when the device has connectivity.

For the problem Mimir is responding to, see `docs/product/01-problem.md`. For the boundaries of v1, see `docs/product/07-scope.md`.
