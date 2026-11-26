# Drill 4: `$groupidentify` is nearly half of total event volume

**My diagnosis:**
The `/habits` page calls `posthog.group('workspace', ...)` with the exact same group properties object inside a recurring background timer, generating a billable `$groupidentify` event every few seconds while the tab sits open.

**My reply to the customer:**
Hey team — `$groupidentify` is the internal event PostHog emits whenever `posthog.group()` is called with a properties object to create or update workspace/company attributes. Looking at your live stream, your `/habits` page is calling `posthog.group('workspace', ...)` with the exact same properties inside a recurring background timer while the tab sits open—and because each property-bearing `group()` call generates a billable event, that loop is inflating your volume. You can fix this immediately by removing `posthog.group()` from that recurring timer and calling it only once at login/signup (or calling `posthog.group('workspace', key)` without the properties object when simply associating events). Follow our [Group Analytics Guide](https://posthog.com) to associate groups without redundant property updates.

**After reading the answer key, what I missed or would change:**
Clarify the exact SDK distinction: calling `posthog.group(type, key)` without a third properties argument attaches `$groups` to subsequent events at zero extra event cost, whereas passing properties emits a separate `$groupidentify` event every time.
