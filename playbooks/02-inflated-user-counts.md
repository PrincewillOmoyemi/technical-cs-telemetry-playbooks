# Drill 2: Tens of thousands of users vs. 3k database accounts

**My diagnosis:**
The initialization code calls `posthog.identify('visitor-' + random)` on every page load, minting a brand-new "identified" person profile on every single visit instead of letting anonymous visitors remain anonymous.

**My reply to the customer:**
Hey team — neither number is lying, they are just measuring different things right now: your site's initialization code is calling `posthog.identify()` with a randomly generated `visitor-<random>` ID on every page load, so every single visit creates a brand-new "identified" user in PostHog—whereas your database counts ~3k real accounts. Removing that `identify()` call from page initialization so it only runs when someone actually authenticates (using a stable account email or database ID) will let anonymous visitors stay anonymous and keep your user counts aligned going forward. Follow our [Identifying Users Guide](https://posthog.com) to wire up authentication cleanly.

**After reading the answer key, what I missed or would change:**
Initially suspected `posthog.reset()` on load because each refresh generated a new ID; inspecting the `distinct_id` format (`visitor-...`) in Persons confirms it was actually `identify()` called with a random string on init—which also creates expensive identified person profiles (~4x cost) for anonymous traffic.
