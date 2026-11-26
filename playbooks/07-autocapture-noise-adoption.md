# Drill 7: Event feed feels like unusable autocapture noise

**My diagnosis:**
Nothing is technically broken—autocapture is active and custom events were never instrumented (or were stripped out), leaving 100% raw `$autocapture` and `$pageview` events with zero defined Actions or named business milestones.

**My reply to the customer:**
Hey team — nothing is broken in your setup, what's missing is a semantic layer on top of raw capture: right now your feed is almost entirely raw `$autocapture` button clicks and `$pageview` events with zero Actions or named custom events to build funnels from. You can unlock immediate value today with zero code changes by defining **Actions** in Data Management for your key clicks—Actions apply retroactively to the 6 months of autocapture data you already have—while your engineers instrument clean `posthog.capture()` events at your 3–5 core lifecycle moments (signup, activation, invite, upgrade). Follow our [Complete Guide to Event Tracking](https://posthog.com) to structure a lean event taxonomy.

**After reading the answer key, what I missed or would change:**
Per PostHog's Basic Implementation Review guidelines, autocapture with zero Actions defined is the #1 indicator of low value realization—framing the fix as "5 core events, not 50" keeps the engineering lift small and actionable.
