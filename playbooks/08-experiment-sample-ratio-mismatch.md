# Drill 8: 80/20 experiment split shows skewed exposure numbers

**My diagnosis:**
Sample Ratio Mismatch (SRM) caused by evaluating `upgrade-cta-copy` before `posthog.identify()` runs—visitors are bucketed anonymously under one ID, re-bucketed into a different variant after login, and post-merge ~1 in 8 users carry exposures to both variants (~65/35 observed split vs. 80/20 configured).

**My reply to the customer:**
Hey team — PostHog's traffic allocator isn't broken, but your experiment is hitting a **Sample Ratio Mismatch (SRM)** because the flag is being evaluated before `posthog.identify()` runs. Visitors get bucketed under an anonymous ID first, then re-bucketed into a different variant after login—leaving a batch of merged users exposed to *both* variants and pulling your observed split away from 80/20. Evaluating the flag after `identify()` completes (or bootstrapping flags server-side with the user's known ID) and setting Multiple Variant Handling to "Exclude from analysis" will keep your experiment splits clean. Follow our [Experiment Exposures & SRM Guide](https://posthog.com) to resolve multi-variant exposures.

**After reading the answer key, what I missed or would change:**
Also note why an experiment's results view and a Trends query over `$feature_flag_called` can disagree: Experiment results strictly filter exposures to the experiment's active run window, whereas Trends queries all matching events unless filtered by timestamp.
