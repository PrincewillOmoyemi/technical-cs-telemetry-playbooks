# Drill 6: Event volume dropped after cookie-consent update

**My diagnosis:**
Initialization sets `opt_out_capturing_by_default: true` and checks `localStorage` for `'accept'` to re-opt users in, while the cookie banner writes `'accepted'`—so the strings never match and even consenting visitors are never opted back in.

**My reply to the customer:**
Hey team — I tested your site as a brand-new visitor and found a subtle string mismatch in your consent update: your SDK initializes opted-out (`opt_out_capturing_by_default: true`) and checks storage for `'accept'` to re-enable tracking, while your cookie banner stores `'accepted'`. Because those strings never match, no new visitor is ever opted back in—even after clicking Accept. Aligning that string check so `posthog.opt_in_capturing()` fires on consent (and optionally exploring `cookieless_mode: 'on_reject'` for privacy-safe counting without cookies) will restore traffic capture immediately. Follow our [Cookieless Tracking & Consent Guide](https://posthog.com) to configure consent handling cleanly.

**After reading the answer key, what I missed or would change:**
Initially assumed `opt_in_capturing()` wasn't wired at all; reading the consent code revealed the exact `'accept'` vs. `'accepted'` string mismatch, and reminded us to note that missing historical weeks cannot be recovered (so the team should annotate their dashboards).
