# Tickler File

Items to surface proactively based on date or conversation context.

## Format
Each entry has a trigger condition and a status. Surface open items when their trigger matches the current date or topic, then ask Jay if he wants to act on them.

---

## Open

| # | Item | Trigger | Notes |
|---|------|---------|-------|
| 1 | Employee Access Control Matrix needs usability improvements and is incomplete | When discussing access management, AM project, or the Access Control Matrix | Details in `notes/employee-access-control-matrix.md` |
| 2 | Send Salesforce users missing Okta Verify list to Thomas McIntosh | HD-37517 follow-up | File: `inbound/Salesforce_Users_Missing_Okta_Verify.xlsx` — 34 active Salesforce users flagged for missing Okta Verify. **Before sending:** check if any have WebAuthn or 1Password passkeys enrolled — those count as phishing-resistant and would already be compliant. Salesforce enforces phishing-resistant MFA for admins June 22 (sandbox) / July 1 (production). |
| 3 | Things to ask Dean if we can enable | Next conversation with Dean Roman | Running list: **(a)** Okta MCP server — use case presented to Dean, not yet decided; if approved, needs a TOOLS ticket. Key use cases: offboarding automation (live app assignments vs. one-off NHT sub-tickets), live MFA/Okta Verify compliance checks (vs. static exported lists), provisioning error triage. Likely requires security/IT policy review. See `knowledge/current_projects.md`. **(b)** Granola MCP Server — [TOOLS-356](https://intranet.oreilly.com/jira/browse/TOOLS-356) (unassigned, To Do) + [HD-36710](https://intranet.oreilly.com/jira/browse/HD-36710) (assigned to you, Open), both filed 2026-02-12 by Sarah Kim (mgr: Simon Neale), both **145 days stale, no movement**. Use case: connect Granola meeting notes to Claude Code via Granola's OAuth-based MCP server to automate action-item extraction. Free / covered by existing licenses per the TOOLS request. |
| 9 | [HD-37601](https://intranet.oreilly.com/jira/browse/HD-37601) — Granola API access for selected notes | Assigned to you, created 2026-07-07 | Jonathan Hassell wants a scoped personal API key (his notes only) to export selected Granola meeting notes/transcripts into a private summarization workflow — member API key creation currently appears disabled workspace-wide. **Note:** this reads as a routine access request Jay can action directly, not a policy question for Dean — pulled out of item #3's "ask Dean" list. Flag if it should be merged back in. |
| 4 | Ask Cali Bail about WIP manuscripts and ChatGPT | Next conversation with Cali Bail | Angela Rufino (`arufino`) asked whether WIP manuscripts are considered non-restricted content under the ChatGPT usage policy. Dean deferred to Cali Bail on this. The current guidelines don't explicitly address manuscripts — they'd likely fall under "source documents" / proprietary content (restricted), but Cali should confirm and potentially update the Confluence guidelines page (pageId: 197231054). |
| 8 | AI Native outreach — Product department | Work on today (2026-06-30) | Product is next after Sales ✓ and G&A/Operations ✓. Sheet in progress. |
| 12 | On24 GDPR anonymization automation meeting with Rachel James (U&A) | When invite lands / next conversation with Rachel James | Rachel is scheduling a meeting (U&A, Help Desk, Live Events, + Susan Strom as courtesy) to discuss hooking up the Groot → On24 anonymization automation now that Flex 4's work covers non-Groot attendees. Prep: trigger mechanism/auth, sync vs. async confirmation, identity mapping, failure/retry handling, test/pilot plan. See `notes/on24-anonymization-automation.md`. |
| 10 | [HD-37611](https://intranet.oreilly.com/jira/browse/HD-37611) — Gong domain settings change | **Deadline: 2026-08-30** | Dean Roman forwarded a Gong notice: Gong is deprecating the "Additional Domains" feature flag in favor of self-managed Email Aliases. After Aug 30, any user recognized in Gong only via an additional domain (not the primary domain) loses automatic org recognition — could lose access to calls/deals/dashboards. Action needed: (1) confirm in Gong Admin Center → Data Protection & Privacy → Internal Domains whether `oreilly.co.uk` is set up as an additional domain, (2) if so, identify which UK users rely on it and are actually in Gong, (3) add their `@oreilly.co.uk` as an "Additional email address" on their Team Member profile before the deadline. Jay's note: we do have the `oreilly.co.uk` domain, but it's not yet known how many UK users use Gong — needs a headcount check. Currently Open, unassigned. |

---

## Snoozed

_Nothing snoozed._

---

## Done

| # | Item | Resolved | Notes |
|---|------|----------|-------|
| 5 | AM-2379 BrowserStack access changes | 2026-06-22 | Approved and moved from AM to SYSPER project. |
| 6 | AM-2385 DAP UAT access for Clarke Cagle | 2026-06-25 | Already closed when checked — no action needed. |
| 7 | Laptop shipment to China | 2026-06-30 | SYSPER ticket and HD ticket created for Michelle Chen. |
| 11 | AM-2412 Datadog API key for Finance App | 2026-07-09 | Closed, assigned to Dean Roman — no further action needed. |
