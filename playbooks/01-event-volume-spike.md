# Drill 1: Event bill tripled with no new features

**My diagnosis:**
The `/habits` page is firing repeated `$set` (Set person properties) events every few seconds on a background interval loop while the tab sits open, even when no user properties changed. Because person-property updates count as billable events, the polling loop multiplies monthly volume.

**My reply to the customer:**
Hey team — I took a look at your live event stream and found what's driving the volume spike: your `/habits` page is firing repeated **`$set` (Set person properties)** events every few seconds on a background timer while the tab sits open, even when nothing has changed. Because each person-property update counts as a billable event, that polling loop is multiplying your monthly volume. You can resolve this immediately by removing `posthog.setPersonProperties()` (or `posthog.identify()`) from that recurring `setInterval` / render loop so it only fires once when a user's profile or plan actually updates. Follow our [Person Properties Setup Guide](https://posthog.com) to update attributes only when values change.

**After reading the answer key, what I missed or would change:**
Check whether `posthog.identify()` is also called inside the same render/effect loop, and note that explicit `setPersonProperties()` calls in a timer bypass SDK deduplication and bill on every tick.
