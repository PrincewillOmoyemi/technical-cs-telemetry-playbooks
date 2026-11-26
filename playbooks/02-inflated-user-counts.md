# Drill 2: Tens of thousands of users vs. 3k database accounts

**My diagnosis:**
The app calls `posthog.reset()` on every page load / SDK initialization, wiping the visitor's cookie and minting a brand-new anonymous `distinct_id` on every refresh. One human reloading several times looks like dozens of separate anonymous profiles.

**My reply to the customer:**
Hey team — neither number is lying, they are just measuring different things right now: your database counts unique accounts (3k), while PostHog is counting fragmented anonymous sessions because **`posthog.reset()` is being called on every page load / SDK initialization**. Every time a visitor refreshes or navigates, `reset()` wipes their cookie and mints a brand-new anonymous `distinct_id`—splitting one person into dozens of profiles and preventing their pre-signup history from merging when they log in. You can fix this by removing `posthog.reset()` from your initialization/layout code and calling it **only** inside your explicit logout handler. Follow our [Resetting a User Guide](https://posthog.com) to wire `reset()` cleanly.

**After reading the answer key, what I missed or would change:**
Emphasize that calling `reset()` on page load also breaks pre-signup-to-post-signup funnel attribution because the anonymous ID is wiped before `identify()` can alias it.
