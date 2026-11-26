# Drill 12: Personal API key (`phx_`) exposed in client-side JavaScript

**My diagnosis:**
Searching the shipped `/upgrade` bundle in DevTools reveals a `phx_` Personal API Key compiled into client-side JavaScript (via `NEXT_PUBLIC_` or importing server-only evaluation code into a client component).

**My reply to the customer:**
Hey team — this is a high-severity leak that needs immediate action: your **`phx_` Personal API Key** grants full read/write administrative access to query project data, modify feature flags, and act on behalf of the team member who created it. Please **revoke and delete that `phx_` key immediately** under **Settings → Personal API keys** and generate a new one—you do **not** need to rotate your `phc_` Project API key, which is write-only and designed to live safely in public browser code. To prevent the new `phx_` key from leaking again, ensure it is never prefixed with `NEXT_PUBLIC_` and move any `posthog-node` local evaluation calls out of `'use client'` components into a server-only API route. Follow our [Private Endpoint Authentication & API Keys Guide](https://posthog.com) to secure your keys.

**After reading the answer key, what I missed or would change:**
Initially mixed up `phc_` vs. `phx_` when identifying which key to rotate—critical lesson: `phx_` is the private admin secret that **must** be rotated immediately, while `phc_` is the public write-only project token that stays untouched. Also advise checking PostHog's Audit Logs for any unauthorized API reads made prior to revocation.
