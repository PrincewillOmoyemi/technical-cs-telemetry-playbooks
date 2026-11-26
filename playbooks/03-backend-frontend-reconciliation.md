# Drill 3: Backend check-ins don't reconcile with frontend users

**My diagnosis:**
Server-side `habit_checked_in` events are arriving in PostHog with zero data loss, but the backend captures every check-in under a single hardcoded service identity (`distinctId: 'hoghabits-backend'`) instead of the acting user's ID.

**My reply to the customer:**
Hey team — good news first: zero data is being dropped. All of your `habit_checked_in` events are arriving in PostHog cleanly, but your backend is capturing them under a single hardcoded **`distinctId: 'hoghabits-backend'`** rather than the acting user's ID, so check-ins pile up on one synthetic service profile instead of individual users. Passing the same user identifier your frontend uses (email or database user ID) into your server-side `capture()` calls will stitch both halves of the journey onto one timeline going forward. Follow our [Node.js Server-Side Capture Guide](https://posthog.com) to pass user IDs consistently.

**After reading the answer key, what I missed or would change:**
Clarify up front that historical events captured under `'hoghabits-backend'` cannot be re-attributed retroactively across individual users—and never merge `'hoghabits-backend'` into a real user, as that would collapse all users together.
