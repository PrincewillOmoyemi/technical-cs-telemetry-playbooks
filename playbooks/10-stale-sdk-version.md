# Drill 10: SDK behaves like older version despite changelog fixes

**My diagnosis:**
Checking `$lib_version` on incoming events shows the app is pinned to `posthog-js` version `1.200.0` via npm rather than loading dynamically from PostHog's CDN snippet, so newer SDK bug fixes aren't in their build.

**My reply to the customer:**
Hey team — we ship every SDK fix to all customers simultaneously, but I checked your incoming events and saw your app is still sending telemetry from **`posthog-js` version `1.200.0`**. Because `posthog-js` is bundled directly inside your application via npm rather than loaded dynamically from our CDN snippet, your build stays pinned to that older version until your team updates the dependency. Upgrading to the latest release (`npm install posthog-js@latest`, plus `posthog-node@latest` if you use our server SDK) and redeploying will bring in all of those fixes immediately. Follow our [JavaScript Web SDK Installation & Upgrade Guide](https://posthog.com) to update cleanly.

**After reading the answer key, what I missed or would change:**
Note that when upgrading across major `posthog-js` versions, they should also review their `defaults` date parameter in `posthog.init()` so they opt into updated default behaviors intentionally.
