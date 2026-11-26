# Drill 9: Agency audit says ~30% of traffic is blocked by ad-blockers

**My diagnosis:**
Inspecting `api_host` in the SDK config confirms the browser is sending telemetry directly to `https://posthog.com` rather than a first-party reverse proxy route, allowing browser ad-blockers and privacy shields to drop ~20–30% of requests.

**My reply to the customer:**
Hey team — the agency's audit is right: I checked your SDK initialization and confirmed your `api_host` is currently pointing directly to **`https://posthog.com`**, which allows browser ad-blockers and privacy shields to intercept and drop roughly 20–30% of client-side analytics requests. You can recover that missing traffic cleanly by setting up a **Reverse Proxy** on your own domain (e.g., routing `/ingest` through your Next.js rewrites, Cloudflare Worker, or CDN to PostHog) and updating your client initialization to `api_host: '/ingest'` with `ui_host: 'https://posthog.com'`. Follow our [Reverse Proxy Setup Guide](https://posthog.com) for copy-paste proxy configurations.

**After reading the answer key, what I missed or would change:**
Remind their engineering team that both the client `api_host: '/ingest'` update AND the server/CDN rewrite rule (including `/ingest/static/*` plus `skipTrailingSlashRedirect: true` in Next.js) must ship together, otherwise every ingestion call will 404.
