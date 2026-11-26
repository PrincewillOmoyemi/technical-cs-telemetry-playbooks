# Drill 7: Event feed feels like unusable autocapture noise

**My diagnosis:**
The frontend relies almost exclusively on raw `$autocapture` DOM click events and `$pageview` without defining Actions or instrumenting named custom lifecycle events (`habit_created`, `teammate_invited`, `upgrade_clicked`).

**My reply to the customer:**
Hey team — I looked at your live event stream and spotted why it feels noisy: almost your entire frontend is relying on raw **`$autocapture` DOM clicks** and `$pageview` events without named custom events or defined Actions. Autocapture is great as a safety net, but without naming your core milestones (`workspace_created`, `habit_created`, `teammate_invited`, `upgrade_clicked`), every chart looks like generic button clicks. The fastest way to get immediate value today—with zero code changes—is to define **Actions** in Data Management for your key buttons; Actions are retroactive, so your past 6 months of autocapture will immediately populate funnels while your engineers add clean `posthog.capture()` events for your top 4–5 lifecycle milestones. Follow our [Actions Setup Guide](https://posthog.com) to label your key interactions retroactively.

**After reading the answer key, what I missed or would change:**
Also recommend enabling "Filter out internal and test users" and building a focused 4-chart Core Activation Dashboard so the team stops reading the raw live Activity firehose as their primary analytics view.
