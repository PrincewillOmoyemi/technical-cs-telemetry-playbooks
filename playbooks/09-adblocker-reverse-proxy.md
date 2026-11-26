# Drill 9: Agency audit says ~30% of traffic is blocked by ad-blockers

**My diagnosis:**
Inspecting `api_host` via Session Replay Doctor (`Activity → Inspector → Doctor → config`) confirms the SDK sends telemetry directly to `https://us.i.posthog.com` rather than a first-party reverse proxy, allowing browser ad-blockers to drop ~20–30% of requests.

**My reply to the customer:**
Hey team — the agency's audit is right, and it's straightforward to fix: your SDK `api_host` currently sends events directly to **`https://us.i.posthog.com`**, which browser ad-blockers block on sight. Routing requests through your own domain using a **Reverse Proxy** (either via Next.js rewrites at `/ingest` or PostHog's Managed Reverse Proxy) makes telemetry first-party traffic and typically recovers 10–30% of blocked volume. Follow our [Reverse Proxy Deployment Guide](https://posthog.com) (or [Managed Reverse Proxy Docs](https://posthog.com/managed-reverse-proxy)) to set it up, and add a PostHog annotation on launch day so the traffic step-up isn't mistaken for an organic spike.

**After reading the answer key, what I missed or would change:**
Include both deployment paths (PostHog's zero-maintenance Managed Reverse Proxy vs. self-hosted Next.js/Cloudflare rewrites) and recommend adding a PostHog Annotation when the proxy ships so analytics teams account for the sudden step-up in volume.
