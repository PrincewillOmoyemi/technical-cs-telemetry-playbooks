# Drill 6: Event volume dropped after cookie-consent update

**My diagnosis:**
The cookie-consent update sets `opt_out_capturing_by_default: true` on initialization without calling `posthog.opt_in_capturing()` when visitors accept, blocking all client-side events for brand-new visitors while returning visitors continue firing events from cached storage.

**My reply to the customer:**
Hey team — I tested your site as a brand-new visitor in a fresh session and confirmed why volume dropped: your cookie-consent update sets PostHog to opt out of capturing by default (`opt_out_capturing_by_default: true`), so zero client-side events fire for new visitors unless an explicit opt-in occurs—whereas returning visitors kept firing events from their cached pre-update storage. To restore accurate traffic visibility while respecting consent, make sure your banner's 'Accept' button calls `posthog.opt_in_capturing()`, or enable `cookieless_mode: 'on_reject'` (or `persistence: 'memory'`) so anonymous pageviews are still counted without storing cookies. Follow our [Cookieless Tracking & Consent Guide](https://posthog.com) to wire up consent handling cleanly.

**After reading the answer key, what I missed or would change:**
Highlight why server-side events and Stripe webhooks stayed completely flat while browser events fell off a cliff—pointing out that discrepancy immediately isolates a client-side consent issue rather than a real traffic drop.
