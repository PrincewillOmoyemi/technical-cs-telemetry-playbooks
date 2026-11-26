# Drill 8: 80/20 experiment split shows skewed exposure numbers

**My diagnosis:**
The experiment is hitting a Sample Ratio Mismatch (SRM) because `upgrade-cta-copy` is evaluated before `posthog.identify()` runs (or overridden client-side), bucketing visitors under a temporary anonymous ID and re-bucketing them into a different variant after login.

**My reply to the customer:**
Hey team — PostHog's traffic allocator isn't broken, but your experiment is hitting a **Sample Ratio Mismatch (SRM)** caused by how and when the flag is evaluated. When `upgrade-cta-copy` is evaluated before `posthog.identify()` runs (or when a user switches devices), they first get bucketed under an anonymous ID and then re-bucketed into a different variant once identified—causing single users to log exposures for both variants and pulling your observed split away from 80/20. To fix this, ensure `posthog.identify()` runs before evaluating the experiment flag (or bootstrap the flag on the server using the user's known ID), and avoid changing variant splits mid-flight. Follow our [Experiments Troubleshooting Guide](https://posthog.com) to resolve Sample Ratio Mismatch and multi-variant exposures.

**After reading the answer key, what I missed or would change:**
Mention PostHog's Multiple Variant Handling setting in Experiments ("Exclude from analysis" vs. "Use first seen variant") and note that if an SRM was caused by a mid-flight split edit, resetting/restarting the experiment is cleaner than trusting contaminated buckets.
