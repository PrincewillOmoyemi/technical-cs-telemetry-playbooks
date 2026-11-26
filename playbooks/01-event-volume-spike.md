# Drill 1: Event bill tripled with no new features

**My diagnosis:**
The `/habits` page fires repeated `$set` (Set person properties) events every second while the tab sits open because `posthog.identify()` is placed inside the React component render body alongside a live "active now" clock ticker and a changing `last_active` timestamp.

**My reply to the customer:**
Hey team — I found what's driving the volume spike: your `/habits` page is calling `posthog.identify()` inside the component render path rather than once at login, so every re-render emits another event. Two details worth knowing: the events show up as `$set` rather than `$identify` because PostHog recognizes the user is already identified and falls through to a person-property update, and passing a changing `last_active` timestamp makes every render look like new data—defeating PostHog's built-in duplicate-call deduplication. Moving `identify()` into your login handler and dropping the timestamp from person properties will bring your volume back down immediately. Follow our [Identify Best Practices Guide](https://posthog.com) to keep person updates lean.

**After reading the answer key, what I missed or would change:**
Initially inferred a `setInterval` calling `setPersonProperties()` from seeing `$set` in the Activity feed; inspecting the event properties reveals the true two-layer mechanic: `identify()` in a React render loop falls through to `$set` once identified, and a changing `last_active` timestamp bypasses SDK deduplication.
