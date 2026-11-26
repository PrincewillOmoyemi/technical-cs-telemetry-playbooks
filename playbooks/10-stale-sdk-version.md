# Drill 10: SDK behaves like older version despite changelog fixes

**My diagnosis:**
Inspecting the `$lib_version` column in Activity shows `posthog-js` is pinned to `1.200.0` via npm, leaving the app months behind bug fixes, performance improvements, and updated SDK defaults.

**My reply to the customer:**
Hey team — we ship every SDK fix to all customers simultaneously, but I checked your incoming events and saw your app is pinned to **`posthog-js` `1.200.0`** via npm—so the fixes you're looking for shipped in newer releases. Upgrading to `posthog-js@latest` (and `posthog-node@latest` on the backend) and adopting a monthly SDK-bump habit will bring in all of those fixes immediately. Check the [posthog-js Browser Changelog](https://github.com) for any breaking notes between `1.200.0` and latest before deploying—happy to sanity-check the diff with you.

**After reading the answer key, what I missed or would change:**
Cite PostHog's Health Check benchmark (the primary SDK carrying volume should never be >3 months behind latest) and link directly to the `posthog-js` `CHANGELOG.md` so engineers can review changes between their pinned version and latest.
