# Drill 4: `$groupidentify` is nearly half of total event volume

**My diagnosis:**
The `/habits` page calls `posthog.group()` with a properties object (including a changing timestamp) directly inside the React component render body, emitting a billable `$groupidentify` event on every re-render.

**My reply to the customer:**
Hey team — `$groupidentify` is emitted whenever `posthog.group()` is called with a properties object to create or update workspace attributes. Right now, your `/habits` page is calling `posthog.group()` with properties directly inside the component render body, generating a new `$groupidentify` event on every render even though the session already remembers the active group. Moving that property-bearing `posthog.group()` call out of the render path so it runs once at login (or only when workspace metadata genuinely changes) will cut that extra volume immediately. Follow our [Group Analytics Guide](https://posthog.com) to associate groups cleanly.

**After reading the answer key, what I missed or would change:**
Note that PostHog's Health Check playbook specifically flags `$groupidentify` above 5% of total volume (or >1 `$groupidentify` per session) as an anti-pattern caused by placing `group()` with properties inside render loops.
