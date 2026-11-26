# Drill 12: Personal API key (`phx_`) exposed in client-side JavaScript

**My diagnosis:**
Searching the shipped `.next/static/chunks/` bundle in DevTools reveals a `phx_` Personal API Key compiled into a client component—granting full read/write administrative permissions to anyone inspecting the browser bundle.

**My reply to the customer:**
Hey team — the researcher is right and this needs immediate action today. Two different PostHog keys exist: your `phc_` Project API key is write-only and safe in browser code by design, but what's compiled into your client bundle is a **`phx_` Personal API Key**, which can read and modify your PostHog project with the key owner's permissions. Please [revoke and rotate that `phx_` key immediately](https://posthog.com) under **Settings → Personal API keys**, and move the server-side call into a backend API route using a server-only environment variable (never prefixed with `NEXT_PUBLIC_`, which inlines secrets into client bundles). We can also help scope the replacement key strictly to the minimum required permissions.

**After reading the answer key, what I missed or would change:**
Initially mixed up `phc_` vs. `phx_` when naming which key to rotate (`phx_` must be rotated immediately; `phc_` stays untouched), and noted why searching the compiled `.next/static/chunks/` bundle in DevTools Sources is unconditional proof compared to watching network requests (which DevTools request-blocking rules can silently suppress).
