# Drill 5: Group analytics enabled, but workspace view is empty

**My diagnosis:**
Enabling Group Analytics in billing/settings turns on the UI schema, but neither the client (`posthog.group()`) nor the server (`groups: { workspace }`) is attaching `$groups` / `$group_0` to outgoing events, so PostHog cannot roll user events up to a workspace.

**My reply to the customer:**
Hey team — I checked your live event stream and found why the Workspace tab is empty: turning on the Group Analytics add-on enables the company view in PostHog, but your incoming events don't have a `$groups` (or `$group_0`) property attached yet, so PostHog can't roll user activity up to a workspace. To populate the workspace view immediately, call `posthog.group('workspace', workspaceId)` on the client right after signup/login so browser events inherit the workspace context, and include `groups: { workspace: workspaceId }` in your server-side `posthog.capture()` calls. Follow our [Group Analytics Setup Guide](https://posthog.com#how-to-set-up-group-analytics) for client and server examples.

**After reading the answer key, what I missed or would change:**
Remind the customer that `posthog-node` on the backend is stateless—unlike `posthog-js` in the browser, every server-side `capture()` call must explicitly pass `groups: { workspace }`.
