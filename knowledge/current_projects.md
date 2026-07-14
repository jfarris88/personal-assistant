# Current Projects

*This file acts as the long-term memory for active projects. Keep this updated as priorities change. Keep each project to a ~10-line summary — move deep detail to a linked `notes/` file.*

## 1. Personal Assistant System (Project Agy)
* **Status:** Active
* **Description:** Setting up Antigravity (Claude Code) to act as a personal executive assistant.
* **Key Goals:** Capture brain dumps, parse tasks/insights, integrate with Jira/Confluence via MCP.
* **Jira:** No dedicated project key yet — track in ITOPS if needed.

## 2. AI Native Initiative — **HOT PRIORITY** (as of 2026-06-23)
* **Status:** Active
* **Description:** O'Reilly is actively working toward becoming an **"AI Native company."** The immediate focus is determining which AI tools each employee needs and ensuring the right people have the right access.
* **Current AI Tool Landscape:**
  * **ChatGPT Enterprise** — assigned to all employees via Okta (company-wide rollout complete as of June 2026)
  * **Gemini** — available as part of the Google Workspace subscription (O'Reilly is a Google shop)
  * **Claude** — partial deployment; engineers and power users; seats managed by Jay
  * **Claude Code** — used by engineering team
  * **Codex, Cursor, Aider, and others** — used by engineers (ad hoc/individual)
* **Active Work:**
  * Reaching out to **department heads** to have them review who on their teams should be assigned Claude. This is the primary near-term action item (as of 2026-06-23).
    * **Sales** — outreach sent ✓
    * **G&A and Operations** — outreach sent ✓; sheet to be shared with Vicky, Becki Valente, and Cali Bail so they can fill in Use Cases tab
    * **Product** — next in queue; sheet in progress; confirmed by Dean as of 2026-06-30
  * A **Use Cases tab** has been added to the tracking sheet so use cases can be listed by Department/Team.
  * Sheet being updated to include a brief summary of what each AI system does (all departments).
  * Managing the Claude rollout: seat procurement, SSO/Okta integration, `orm-claude-antfarm` admin account, and user support. **Currently on Team plan** (144 seats, confirmed 2026-07-10) — Enterprise upgrade being considered/planned but not yet done. Team plan has no custom-role support: only Owner/Primary Owner can see Billing; Admin cannot. Custom roles (billing/analytics view-only without full admin) only become available after moving to Enterprise.
* **Key Goals:** Match AI tool assignments to actual employee needs; ensure Claude seats are provisioned correctly; SSO/Okta integration is stable.
* **Jira:** Tickets tracked across `SYSPER` (seat purchases), `ITOPS` (SSO/infrastructure), `HD` (user support), `AM` (access requests).
* **Active ticket:** `TOOLS-379` — Vercel AI tooling request (In Progress).

## 3. Gong / Amplemarket Integration (HD-37515)
* **Status:** Waiting-on (meeting to be scheduled)
* **Description:** Thomas McIntosh (Salesforce Admin) requested integration between Gong and Amplemarket. Meeting events tracked by Amplemarket that Gong transcribes would have the transcription added to Amplemarket for additional context on companies/leads/contacts.
* **Current State:** Security reviewed and approved (Jeff Powell, June 17). Currently assigned to Michael Seneschal.
* **Next Step:** Jay to coordinate a meeting with **Keith Swafford, Thomas McIntosh, Dean Roman, and Jay** to discuss.
* **Jira:** [HD-37515](https://intranet.oreilly.com/jira/browse/HD-37515)

## 4. IT Operations Day-to-Day
* **Status:** Active (ongoing)
* **Description:** Ongoing IT operations work including help desk support, access management, and GCP IAM support for engineering teams.
* **Key Goals:** Keep the ticket queue moving across HD, AM, and SOL.
* **Jira Projects:**
  * `HD` — Help Desk (general support requests) — primary
  * `AM` — Access Management (provisioning/deprovisioning) — primary
  * `SOL` — Inbound triage queue: anything comes in here first so it can be routed to the right project/team — primary
  * `ITOPS` — IT Operations and Infrastructure Engineering — secondary
  * `NHT` — Offboarding/terminations — ⚠️ **AI ACCESS NOT APPROVED** (sensitive employee data — do not query)

### Offboarding / Deprovisioning Process
When a user leaves, Okta automatically creates tickets to offboard them from SaaS apps. These are generated as sub-tickets attached to the parent `NHT` (offboarding) ticket for that user. AI systems should not access NHT tickets directly.

## 5. Atlassian Cloud Migration
* **Status:** Active
* **Description:** Migrating O'Reilly's Atlassian stack (Jira, Confluence, Crowd, Service Management) to Atlassian cloud. Contract with vendor **Sentify** is about to be signed (as of 2026-06-25). Kickoff will follow once Sentify is on board.
* **Slack Channel:** `#atlassian-cloud-migration` (created June 22, 2026 by Dean Roman)
* **Vendor:** Sentify — vendor has no PjM; O'Reilly must provide their own
* **Internal PjM:** Shawn Storc
* **Phase 1:** Discovery
* **Key Domain Change:** `intranet.oreilly.com` → `oreillymedia.atlassian.net` (or similar; Atlassian doesn't allow custom domains)
* **Key Stakeholders:** Dean Roman (project owner), Shawn Storc (PjM), Jay Farris, Brad Frank
* **Shared Drive:** Set up by Shawn Storc; all channel members have content manager permissions; link in channel bookmarks
* **Reference:** [Atlassian IP addresses and domains for cloud products](https://support.atlassian.com/organization-administration/docs/ip-addresses-and-domains-for-atlassian-cloud-products/) — needed for firewall/network allowlist work
* **Jira:** No project key assigned yet
* **Future:** Plan to stand up a **fresh Confluence space for the IT/Helpdesk team's internal documentation** as part of the cloud migration — clean slate rather than migrating legacy content. Several docs currently scattered in Google Docs and legacy Confluence Data Center pages (e.g., [create_ro_database_user.sh runbook](https://docs.google.com/document/d/1XgwrQGx0ucy1gEMSzPcFXjAl-CgNQNweh08TycWZQVA/edit?tab=t.0), [Adding SSH keys for Linux system authentication](https://intranet.oreilly.com/confluence/pages/viewpage.action?pageId=79135041)) should be relocated there once the space exists. Running tracker: [confluence-cloud-migration-candidates.md](../notes/confluence-cloud-migration-candidates.md).

## 6. MCP Server Infrastructure
* **Status:** Background (mostly closed out)
* **Description:** Building and maintaining MCP (Model Context Protocol) server infrastructure, including a GCP VM in the Helpdesk project and Okta service account integration for Cypress.
* **Key Goals:** Stable MCP server environment for AI tooling; Okta/SSO service account for `mcp` is approved and in progress.
* **Jira:** `AM-2364` (Cypress okta service account — **Closed** as of 2026-06-25), `ITOPS-38920` (GCP VM — Closed).

### 💡 Future: Okta MCP Server Connection
Need to ask whether we can connect an Okta MCP server to the AI assistant environment. This would open up automation opportunities — particularly around **offboarding/deprovisioning** (currently handled via Okta tasks generating NHT sub-tickets). Approval needed before pursuing; likely involves security/IT policy review.
* **Potential use cases (specific to our environment):**
  * **Offboarding automation** — query a departing employee's live Okta app assignments directly instead of working NHT sub-tickets one SaaS app at a time; pre-flag or auto-action deprovisioning steps.
  * **MFA/Okta Verify compliance checks** — pull live phishing-resistant MFA enrollment status (WebAuthn/passkey/Okta Verify) per app on demand, instead of one-off exported lists (e.g. the Salesforce Okta Verify spreadsheet from HD-37517) — reusable for future enforcement waves on other apps.
  * **Provisioning error triage** — look up a user's Okta profile/assignment errors directly (e.g. the RingCentral UK-number provisioning error for a recent new hire, raised in standup) instead of manual digging in the Okta admin console.
* Raised with Dean in 1-1 on 2026-07-02 — see [daily/2026-07-02.md](../daily/2026-07-02.md). Also tracked on the "ask Dean" running list — see `tickler.md`.

## 7. Finance Dashboard — "OpEx Command Center"
* **Status:** Active
* **Description:** Internal Finance reporting dashboard built by **Michael Trice** (Senior Financial Analyst) to replace the team's manual Google Sheets reporting process. Also referred to internally as the "Finance Dashboard" or "finance app."
* **Next Step:** No open action for Jay currently — access/infra requests are progressing through their Jira trail.
* **Details:** Stack, hosting, auth, and full Jira trail moved to [finance-dashboard-opex.md](../notes/finance-dashboard-opex.md).
* **Jira:** Latest — [AM-2412](https://intranet.oreilly.com/jira/browse/AM-2412) (Datadog API key, current as of 2026-07-08). Full trail in the linked note.

## 8. ProQuest Mulesoft API — Order Posting Integration
* **Status:** Active
* **Description:** Building an external API in **Mulesoft Anypoint** for vendor **ProQuest** to post orders into O'Reilly's Salesforce instance. Currently securing the API endpoints (raised by the Salesforce admin, 2026-07-06).
* **Auth options considered:**
  * **Okta as OAuth 2.0 IdP** — ruled out; O'Reilly's Okta tenant is workforce/internal-only, not appropriate for authenticating an external vendor's server-to-server calls (no separate B2B/customer-identity org exists for this purpose).
  * **Salesforce as OAuth 2.0 IdP** — ruled out; would require standing up an external client app + dedicated API user in Salesforce and handing those credentials to the vendor, effectively granting direct (if permission-scoped) org access instead of just the API surface.
  * **Anypoint Basic Client ID Enforcement** — native to Anypoint, no external IdP needed, static non-expiring `client_id`/`client_secret` sent per request.
  * **OAuth 2.0 client credentials via a self-hosted Mule OAuth 2.0 Provider app** — Mulesoft can act as its own standards-based token issuer, fully decoupled from Okta/Salesforce, giving short-lived scoped tokens — more setup/maintenance than the option above.
* **Recommendation given (2026-07-06):** For this single-vendor, machine-to-machine order-posting use case, Basic Client ID Enforcement + compensating controls (mutual/two-way TLS, IP allowlist restricted to ProQuest's egress IPs, rate limiting/throttling policy) is a pragmatic fit — avoid exposing Okta or Salesforce to an external party. If O'Reilly expects to onboard more external partners this way, revisit and invest in the self-hosted Mule OAuth 2.0 Provider for real token expiry/revocation/scopes.
* **Jira:** none yet

## 9. On24 GDPR Anonymization Automation
* **Status:** Waiting-on (meeting to be scheduled)
* **Description:** GDPR PRIV data-deletion requests need to check/anonymize users in **On24** (webinar/live-events platform) and **OLT** (Online Training Slack workspace). Current automation ([gdpr-priv-automation](https://github.com/oreillymedia/gdpr-priv-automation) repo) only checks whether the user still exists and updates the linked Jira ticket — it does not actually anonymize anything, and On24/OLT are not among the repo's automated closers (handled manually today, e.g. [SOL-101033](https://intranet.oreilly.com/jira/browse/SOL-101033)).
* **Development (2026-07-10):** **Flex 4** (external team) has built automation to actually anonymize On24 users, hooking into existing U&A automation. Previously on hold; now resumed and extended to cover **external event attendees (non-Groot users)** — the main previous gap. **Rachel James** (U&A) asked whether Jay is ready to hook up the Groot → On24 automation to this, and is scheduling a meeting with U&A, Help Desk, and Live Events reps (**Susan Strom** added as a courtesy per Shawn Storc).
* **Jay's position:** yes in principle, but need to nail down the integration contract first — trigger mechanism/auth, sync vs. async confirmation, Groot↔On24 identity mapping (esp. for non-Groot attendees), failure/retry handling, and a test/pilot plan before going live on real GDPR requests.
* **Key docs:** Google Doc "On24 Anonymization Rollout" (Jay's copy: id `1BqBnYuVb81O_VgFd-adzJPbHD1nC5t2S3A12WSslsYc`). Full writeup: [on24-anonymization-automation.md](../notes/on24-anonymization-automation.md).
* **Next step:** Attend/prep for the meeting Rachel is scheduling; confirm date once invite lands.
* **Jira:** [SOL-101033](https://intranet.oreilly.com/jira/browse/SOL-101033) (example closed ticket, for reference)

## 10. Helpdesk Standup Notes — Confluence v2.0 (Jay's personal space pilot)
* **Status:** Active
* **Description:** Replacing the running "Helpdesk Standup Notes & Agenda" Google Doc (6+ years of history, ~4,700 lines, one entry per weekday with plain Jira links and freeform notes) with a Confluence **Live page** in Jay's personal Cloud space (space name "Jay Farris", spaceId `13107203` on `oreillymedia.atlassian.net`). Built ahead of the full [Atlassian Cloud Migration](#5-atlassian-cloud-migration) as a personal pilot — not yet the org-wide IT/Helpdesk space mentioned in that section.
* **Source doc:** Google Doc `1jPZ2PzGtiDBMmReDH0vZGLCd7E4Q1Tfo6tBn9ro7D6M` — read via the `gcp-helpdesk-prod-dsadmin@helpdesk-prod-1.iam.gserviceaccount.com` service account (creds in 1Password, Helpdesk vault, item `gcp-helpdesk-prod-dsadmin@...`; the actual JSON key is a **file attachment** on that item, not a text field). Jay shared the doc with that service account directly.
* **Design decisions (confirmed with Jay 2026-07-01):**
  - Live page holds **current/active items only**; resolved/older items get moved to a child "Helpdesk Standup Archive" page (regular, non-live) — not kept forever on the Live page like the old Doc.
  - Daily items shown as a **table** (Status | Item | Notes), not freeform bullets.
  - **Status lozenges** per item (Open/Waiting/Done, etc.) instead of parenthetical notes like "(Follow up)".
  - Jira tickets are **plain hyperlinks** for now (`AM-1234: short title`), not live issue macros — O'Reilly's Jira is still on Data Center (`intranet.oreilly.com/jira`) and not yet connected to the `oreillymedia.atlassian.net` Cloud site, so the native Jira issue macro has nothing to render against. Revisit once the Jira Cloud migration (section 5) lands, or consider the **Jira Data Center Connector** app as a stepping stone if live status is wanted sooner (needs Confluence + Jira admin setup — bigger lift, separate conversation).
  - "Corporate Goals" and "Quick Links" sections kept from the original doc, collapsed via `expand` macros to reduce clutter.
  - Action items tracked as a real Confluence **task list** (not plain bullets).
* **Pages created:**
  - Live page: "Helpdesk Standup" (id `13434881`, parent: "Overview" id `13107295`) — [webui link](https://oreillymedia.atlassian.net/wiki/spaces/~5bf49087da94df18b4d49b9b/pages/13434881/Helpdesk+Standup)
  - Archive child page: "Helpdesk Standup Archive" (id `13664257`, parent: the Live page above)
* **Next Step:** Layout/functionality v1 built and live as of 2026-07-01, seeded with example rows — not yet backfilled with real content from the Doc (Jay said not to pull everything over yet, just get the structure right first).
* **Details:** Confluence Cloud API access technicals for this space moved to [confluence-cloud-api-access.md](../notes/confluence-cloud-api-access.md).
* **Jira:** none
