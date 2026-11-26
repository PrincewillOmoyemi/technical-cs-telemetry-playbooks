# Drill 3: Backend check-ins don't reconcile with frontend users

**My diagnosis:**
Server-side `habit_checked_in` events are arriving in PostHog with zero data loss, but they are all captured under a single hardcoded `distinctId: 'hoghabits-backend'` instead of the authenticated user's email/ID, leaving real user profiles with zero check-in events.

**My reply to the customer:**
Hey team — good news first: zero data is being dropped. All of your `habit_checked_in` events are arriving in PostHog cleanly, but they are being captured under a single hardcoded **`distinctId: 'hoghabits-backend'`** instead of each user's actual identifier. Because your frontend calls `posthog.identify(email)` while your backend logs every check-in under that shared service ID, PostHog treats them as separate profiles. You can resolve this by passing the authenticated user's real identifier (e.g., `distinctId: email` or their database user ID) into your server-side `posthog.capture()` call so backend and frontend events stitch onto the same person. Follow our [Identifying Users Guide](https://posthog.com) to align client and server IDs.

**After reading the answer key, what I missed or would change:**
Note that historical events sent under `'hoghabits-backend'` cannot be split retroactively across individual users—merging `'hoghabits-backend'` would collapse every user into one giant person, so only fix the ID going forward and exclude `'hoghabits-backend'` from user-level insights.
