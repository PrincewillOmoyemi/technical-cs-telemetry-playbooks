# Drill 11: Web analytics only shows one pageview per multi-page visit

**My diagnosis:**
In Next.js, initial page loads fire `$pageview`, while client-side `<Link>` transitions update the URL via the History API without triggering a full page reload, and `capture_pageview` is not set to `'history_change'`.

**My reply to the customer:**
Hey team — I tested your navigation flow and confirmed why Web Analytics only shows one pageview per visit: full browser loads fire `$pageview` normally, while client-side `<Link>` transitions inside Next.js update the URL via the History API without triggering a full page reload, so PostHog never sees subsequent route changes. You can fix this cleanly by setting `capture_pageview: 'history_change'` (or updating your `defaults` date to `'2026-01-30'` without overriding `capture_pageview: false`) inside your `posthog.init()` config so PostHog automatically captures every SPA route transition. Follow our [Next.js Pageview Capture Guide](https://posthog.com) to configure SPA route tracking.

**After reading the answer key, what I missed or would change:**
Warn against adding both `capture_pageview: 'history_change'` AND a manual `usePathname()` `posthog.capture('$pageview')` effect at the same time, as combining both causes double-counted pageviews on every route change.
