# Drill 11: Web analytics only shows one pageview per multi-page visit

**My diagnosis:**
The `defaults` option was omitted from `posthog.init()`, reverting `posthog-js` to legacy page-load-only `$pageview` capture so Next.js App Router History API transitions produce zero pageviews.

**My reply to the customer:**
Hey team — your site is a single-page app: after the initial page load, Next.js navigates via the browser History API without full page reloads, and your `posthog.init()` config is missing the modern `defaults` option—reverting the SDK to legacy page-load-only pageview capture. Adding the current `defaults` date (or `capture_pageview: 'history_change'`) to `posthog.init()` enables automatic SPA route tracking across every in-app navigation. Follow our [JavaScript Web SDK Config Guide](https://posthog.com) to update your initialization snippet, and add a PostHog annotation when it ships since pageview counts will step up cleanly.

**After reading the answer key, what I missed or would change:**
Pinpointed the exact config regression (`defaults` missing from `posthog.init()`) rather than guessing `capture_pageview: false`, and included the operational reminder to annotate dashboards when SPA pageview capture goes live.
