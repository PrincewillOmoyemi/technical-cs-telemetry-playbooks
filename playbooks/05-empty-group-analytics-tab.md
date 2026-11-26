# Drill 5: Group analytics enabled, but workspace view is empty

**My diagnosis:**
Group analytics is enabled on the account, but none of the three required group wires exist in code: no client `posthog.group()` on login, no server `groupIdentify()`, and no per-event `groups` parameter on server-side `capture()` calls.

**My reply to the customer:**
Hey team — Group Analytics is enabled on your account, but your SDKs aren't attaching group context to events yet, so PostHog has nothing to aggregate in the Workspace tab. It needs two small additions: call `posthog.group('workspace', workspaceId)` in the browser when a user logs in (which links all subsequent session events), and pass `groups: { workspace: workspaceId }` on every server-side `capture()` call (since the server SDK is stateless between requests). Follow our [Group Analytics Setup Guide](https://posthog.com) for exact client and server snippets.

**After reading the answer key, what I missed or would change:**
Always verify event properties (`$groups` / `$group_0`) before replying: events lacking `$groups` means missing code instrumentation (a support fix), whereas events carrying `$groups` with a locked/empty tab means the Group Analytics add-on isn't enabled yet (a billing conversation).
