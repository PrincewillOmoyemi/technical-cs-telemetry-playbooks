# Technical CS Telemetry & Implementation Playbooks

A field-tested library of **12 production telemetry diagnostic playbooks** and a **B2B Implementation Health Check Matrix** built for **Technical Customer Success Managers (TCSMs)**, **Customer Success Engineers (CSEs)**, and **Solutions Engineers**.

Every playbook in this repository was diagnosed live against a full-stack Next.js + Node.js SaaS application (`HogHabits`) instrumented with client-side (`posthog-js`) and server-side (`posthog-node`) SDKs, reverse proxies, group analytics, feature flags, and A/B experiments.

---

## Why I Built This: Commercial Judgment + AI Execution

Coming from a commercial and sales background, I built this repository to bridge the gap between **customer outcomes** and **technical root-cause analysis**.

Rather than treating technical troubleshooting as a black box, I used an **AI-augmented Director & Doer workflow**—acting as the technical director to reproduce customer symptoms in a live Next.js + Node.js sandbox, inspect event streams and network payloads, validate hypotheses against official documentation, and synthesize blameless, engineer-ready resolutions.

This mirrors how modern Technical Customer Success operates: combining commercial empathy and problem framing with AI-accelerated diagnostics to resolve complex blockers in minutes rather than days.

---

## Diagnostic Methodology

Every customer ticket follows a structured four-step triage framework:
1. **Reproduce & Isolate:** Verify whether the anomaly originates in client-side rendering, server-side event capture, network routing, or statistical bucketing.
2. **Evidence-First Verification:** Confirm root cause directly in live event streams, person/group profiles, SDK metadata (`$lib_version`, `api_host`), and browser bundles before proposing code changes.
3. **Blameless Customer Communication:** Deliver concise, non-defensive Slack responses explaining *what* is happening, *why* it affects billing or data integrity, and *how* to fix it.
4. **Documentation-Backed Resolution:** Pair every fix with canonical SDK and architecture documentation.

---

## 12 Production Telemetry Playbooks

| # | Failure Mode / Customer Symptom | Root Cause Isolated | Playbook |
| :--- | :--- | :--- | :--- |
| **01** | Event bill tripled with no new features | Recurring `setInterval` firing `$set` person-property updates | [View Playbook](./playbooks/01-event-volume-spike.md) |
| **02** | 40k+ analytics users vs. 3k DB accounts | `posthog.reset()` called on page load/initialization | [View Playbook](./playbooks/02-inflated-user-counts.md) |
| **03** | Backend check-ins missing from user profiles | Shared hardcoded `distinctId` on server `capture()` calls | [View Playbook](./playbooks/03-backend-frontend-reconciliation.md) |
| **04** | `$groupidentify` is ~50% of total event volume | `posthog.group()` with properties called inside a polling loop | [View Playbook](./playbooks/04-high-groupidentify-volume.md) |
| **05** | Group analytics enabled, workspace view empty | Missing `$groups` / `groups` context on client & server events | [View Playbook](./playbooks/05-empty-group-analytics-tab.md) |
| **06** | Traffic dropped off a cliff after consent banner update | `opt_out_capturing_by_default: true` without `opt_in_capturing()` | [View Playbook](./playbooks/06-cookie-consent-traffic-drop.md) |
| **07** | Event feed feels like unusable click noise | Raw `$autocapture` only; zero Actions or named lifecycle events | [View Playbook](./playbooks/07-autocapture-noise-adoption.md) |
| **08** | 80/20 A/B experiment split shows skewed exposure counts | Sample Ratio Mismatch (SRM) from pre-`identify()` flag evaluation | [View Playbook](./playbooks/08-experiment-sample-ratio-mismatch.md) |
| **09** | Agency audit shows ~30% traffic blocked by ad-blockers | Direct ingestion to `us.i.posthog.com` instead of first-party proxy | [View Playbook](./playbooks/09-adblocker-reverse-proxy.md) |
| **10** | Customer still hitting resolved SDK bugs | Pinned `posthog-js` `1.200.0` dependency via npm | [View Playbook](./playbooks/10-stale-sdk-version.md) |
| **11** | Web analytics records only 1 pageview per multi-page visit | Next.js SPA History API transitions uncaptured | [View Playbook](./playbooks/11-nextjs-spa-pageviews.md) |
| **12** | Personal API key (`phx_`) leaked in browser JS bundle | Secret compiled into client component / `NEXT_PUBLIC_` env var | [View Playbook](./playbooks/12-exposed-personal-api-key.md) |

---

## Additional Reference

* **[B2B Implementation Health Check Matrix](./B2B-IMPLEMENTATION-CHECKLIST.md):** Pre-call technical audit checklist covering ingestion proxies, identity stitching, B2B group schemas, flag hydration/bootstrapping, and security hygiene.
