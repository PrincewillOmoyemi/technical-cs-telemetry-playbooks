# B2B Implementation Health Check Matrix

Pre-call technical audit framework used prior to customer discovery, QBRs, and expansion reviews.

| Audit Domain | What to Inspect | Healthy State | Red Flag / Action Item |
| :--- | :--- | :--- | :--- |
| **Ingestion & Ad-Blockers** | Network tab / Replay Doctor `api_host` | First-party reverse proxy (`/ingest`) + `ui_host` configured | Direct calls to `*.i.posthog.com` (~20–30% traffic loss) |
| **Identity Resolution** | `Persons` & `Activity` `distinct_id` continuity | Anonymous IDs merge cleanly into DB user IDs at login; `reset()` only on logout | Exploding anonymous IDs or hardcoded backend `distinctId` |
| **Billing & Volume Hygiene** | Event breakdown (`$set`, `$groupidentify`, `$autocapture`) | Property updates fire only on state change | Polling timers firing `$set` or `$groupidentify` continuously |
| **B2B Group Analytics** | Event `$groups` property & Group Types | Client `posthog.group()` + stateless server `groups: {}` aligned | Empty Workspace tab or >5 group types configured |
| **SPA Pageviews** | Multi-page session timelines | `capture_pageview: 'history_change'` (or current `defaults`) active | Single `$pageview` per session on Next.js/React routes |
| **Feature Flags & Experiments** | Flag evaluation timing & SRM checks | Server-side local evaluation / bootstrapping; `identify()` before experiment exposure | Hydration mismatches, pop-in flicker, or Sample Ratio Mismatch |
| **SDK Currency** | Event `$lib_version` property | Recent `posthog-js` and `posthog-node` versions | Pinned legacy npm versions missing bug fixes |
| **Security & Key Hygiene** | Client JS bundle & `.env` prefixes | Public `phc_` token in client; `phx_` Personal API keys strictly server-side | Any `phx_` key exposed in browser bundles or `NEXT_PUBLIC_` |
