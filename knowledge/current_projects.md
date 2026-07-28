# Current Projects

*This file acts as the long-term memory for active projects. Keep this updated as priorities change. Keep each project to a ~10-line summary — move deep detail to a linked `notes/` file.*

## 1. Personal Assistant System (Project Agy)
* **Status:** Active
* **Description:** Setting up Antigravity (Claude Code) to act as a personal executive assistant.
* **Key Goals:** Capture brain dumps, parse tasks/insights, integrate with Jira/Confluence via MCP.
* **Jira:** No dedicated project key yet — track in ITOPS if needed.

## X. Salesforce Slackbot Integration ([HD-37652](https://intranet.oreilly.com/jira/browse/HD-37652))
* **Status:** Open, assigned to Jay
* **Description:** Heather Polzin (Director, Global Sales Development) requested the official Salesforce app for Slack be turned on, for improved reporting/visibility into team performance and pipeline. As of 2026-07-17, **the Salesforce Slack app is confirmed not currently installed** in the O'Reilly Slack workspace (Jay checked directly).
* **Related prior ticket:** [TOOLS-53](https://intranet.oreilly.com/jira/browse/TOOLS-53) (2021, Done) — narrower scope: enabled the same Salesforce Slack app against the **Salesforce Sandbox** (not production) for Opportunity-Won notifications for the UK Rights team only. Does not cover Heather's ask; unclear if that install is even still active.
* **Update (2026-07-17, Slack w/ Thomas McIntosh):** Thomas confirmed strong interest org-wide — framed Slack (a Salesforce company) as a **more secure path to Salesforce data than MCP/Claude**. Jay agrees this integration is the better approach vs. querying Salesforce through Claude. **Thomas will file the [Tools and Services Request](https://intranet.oreilly.com/confluence/display/IS/Tools+and+Services+Request)** (Heather confirmed "perfect" with Thomas filing on her behalf rather than her doing it). Thomas should also attend the Tools review meeting to answer questions.
* **Next Step:** Waiting on Thomas to file the Tools request; Jay to ensure Thomas is included in the Tools meeting when scheduled.
* **Jira:** [HD-37652](https://intranet.oreilly.com/jira/browse/HD-37652)

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
  * Building an **AI usage and adoption dashboard** — pulling in usage/analytics data across the org's AI tools. OpenAI admin analytics console: https://admin.openai.com/analytics (source for ChatGPT Enterprise usage data).
  * Reaching out to **department heads** to have them review who on their teams should be assigned Claude. This is the primary near-term action item (as of 2026-06-23).
    * **Sales** — outreach sent ✓
    * **G&A and Operations** — outreach sent ✓; sheet to be shared with Vicky, Becki Valente, and Cali Bail so they can fill in Use Cases tab
    * **Product** — next in queue; sheet in progress; confirmed by Dean as of 2026-06-30. **Resumed 2026-07-23** (had stalled 3+ weeks with no logged update): working copy sheet located ([reference-documents.md](reference-documents.md)) and a first-draft **"Product Management" tab** added, listing all 38 Product Management-department employees (per the HRIS "Latest Roster" tab) grouped into 7 team sections by manager — Product Design (Boutté), Instructional/Content Design (Gonzalez), Data Science (King), two PM teams (Manning, Neale), Principal PM (Tongsinoon), and Project Management & QA (Paterson). Rough draft only — next step is deciding the final per-department sheet format (Jay's plan: break Product out into its own sheet with team sub-tabs, mirroring the Sales Teams template) and adding the AI-tool-assignment columns like the Accounting/Legal/HR tabs have.
  * A **Use Cases tab** has been added to the tracking sheet so use cases can be listed by Department/Team.
  * Sheet being updated to include a brief summary of what each AI system does (all departments).
  * Managing the Claude rollout: seat procurement, SSO/Okta integration, `orm-claude-antfarm` admin account, and user support. **Currently on Team plan** (144 seats, confirmed 2026-07-10) — Team plan has no custom-role support: only Owner/Primary Owner can see Billing; Admin cannot. Custom roles (billing/analytics view-only without full admin) only become available after moving to Enterprise.
  * **Claude token spend tips doc** — Sanjay Khona shared a WIP doc (Slack, 2026-07-22) with tips for managing/optimizing Claude token spend, from his own experience: https://claude.ai/code/artifact/c61878df-481e-42bb-8e59-277113563b58. Timed to the Enterprise migration scramble; unreviewed/informal but Jay flagged it as good.
  * **Enterprise upgrade — completed (confirmed 2026-07-22).** Account has migrated from Team to Enterprise. Known side-effects found so far: (1) **Claude in Chrome is disabled by default on Enterprise** (was on-by-default on Team) — an Owner/Primary Owner must re-enable it via Org settings > Claude in Chrome > toggle on; no data is lost, it's a policy default, not a bug. (2) SCIM group mapping (Okta → Claude groups) is now in use — see SCIM/Admin-API note below.
  * **SCIM group mapping (Okta → Claude), set up 2026-07-22.** Jay configured Okta push + SCIM group mappings for 5 Claude groups (Claude Owner/Engineering/Product/Sales/Users). The "Confirm group mappings" dialog only shows counts ("remove N, add N"), never names — no in-product preview of *which* user. Workaround: pull the live Claude org member list via the Admin API and diff against the Okta group export (see [Claude Admin/Compliance API](reference-documents.md) entry). **Verified 2026-07-22:** cross-checked all 195 Claude org members against the "Claude All AD Groups" tab of the Ad Hoc userlist sheet — 0 add / 0 remove, fully in sync.
  * **Individual API-account users being migrated to Enterprise seats (raised 2026-07-24, re: Kelvin Hammond).** Jay's messaging to people currently on personal Claude API accounts: Enterprise gives them the desktop app (chat/Cowork/Claude Code) plus CLI access, all authenticated via Okta, for centralized management — API accounts are being kept only for service-to-service use, not individual usage. Billing dashboard (Console > Usage) also confirmed to show a **"Discounted" vs "List price" vs "Billed"** toggle, confirming O'Reilly's Enterprise spend is at a negotiated discount off list price — exact discount % not confirmed, would need contract/rate card.
  * **Enterprise upgrade — seat count planning (from 2026-07-16, still open).** Dean Roman told Jay and Jamey Harvey (Slack, 2026-07-16 ~8am) he signed the Enterprise Agreement; asked Baaqir Yusuf (Anthropic contact) what changes after signing — Dean doesn't expect anyone to lose access, expects more user-management options. Dean asked Jay: (1) how many Claude users are needed based on the "AI Strategy for [Department]" sheets teams have been filling out, (2) what other teams still need to be accounted for, (3) whether 180 seats is enough to start (can increase anytime, so a rough number is fine — Dean wants to sign same-day). Jay's response/plan: only Sales and Finance/Accounting have been fully distributed the AI strategy sheet so far (Product already has broad Claude access separately); Jay will (a) pull the list of pending Claude access requests organized by department, (b) organize existing subscription users by department (see [Claude Subscriptions tab](https://docs.google.com/spreadsheets/d/1qV46CW_vHH9YtCr0pIsY77tK7cIxnCPB_TJKvLNmOP0) in the Ad Hoc userlist sheet — kept current via CSV export cross-referenced against the roster), and (c) compile Sales/Finance users specifically from the AI strategy sheets, to build a rough total seat count. **No action yet — wait for Jay's go-ahead before compiling/sending anything.**
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
* **Infra ownership (corrected 2026-07-17):** The on-prem/DC Atlassian stack (Jira, Confluence, Crowd) is administered by **ITOps**, not Platform Engineering (PE). Brad Frank (PE) is a migration stakeholder/channel member but PE does not own the DC instance. Don't route DC-administration questions (webhooks, marketplace apps, sandbox refresh, admin access) to PE by default — route to ITOps.
* **DC sandbox:** Confirmed to exist at `intranet-dev.oreilly.com` (mirrors prod `intranet.oreilly.com`; Jira reachable at `/jira`). Referenced in [HD-32204](https://intranet.oreilly.com/jira/browse/HD-32204) (2023) — Justin Breninger confirmed the sandbox's Jira API at the time. Whether it's been refreshed with current production data recently is unconfirmed as of 2026-07-17.
* **Shared Drive:** Set up by Shawn Storc; all channel members have content manager permissions; link in channel bookmarks
* **Reference:** [Atlassian IP addresses and domains for cloud products](https://support.atlassian.com/organization-administration/docs/ip-addresses-and-domains-for-atlassian-cloud-products/) — needed for firewall/network allowlist work
* **Jira:** No project key assigned yet
* **Future:** Plan to stand up a **fresh Confluence space for the IT/Helpdesk team's internal documentation** as part of the cloud migration — clean slate rather than migrating legacy content. Several docs currently scattered in Google Docs and legacy Confluence Data Center pages (e.g., [create_ro_database_user.sh runbook](https://docs.google.com/document/d/1XgwrQGx0ucy1gEMSzPcFXjAl-CgNQNweh08TycWZQVA/edit?tab=t.0), [Adding SSH keys for Linux system authentication](https://intranet.oreilly.com/confluence/pages/viewpage.action?pageId=79135041)) should be relocated there once the space exists. Running tracker: [confluence-cloud-migration-candidates.md](../notes/confluence-cloud-migration-candidates.md).
* **Sentify Pre-Kickoff Questions (2026-07-17):** Vendor doc "Copy of Pre-Kickoff Questions" ([Google Doc](https://docs.google.com/document/d/1dWSKp3qpWZY1mJHvlb1cf_xb2ojw-fe8hDsEJ-Wjnj4/edit), shared with `gcp-helpdesk-prod-dsadmin@oreilly.com`) — answers due before the **8/3 kickoff**. Five questions: (1) review shared Jira/Confluence marketplace apps — move to Cloud or retire, plus business priority/usage for each; (2) list/confirm usage/priority of the 37 webhooks previously reported; (3) do we have a Cloud instance already — yes, `oreillymedia.atlassian.net` exists but currently only hosts Jay's personal Confluence space pilot (see section 10), not a full org site; (4) DC sandbox refreshed with current prod data — sandbox exists (`intranet-dev.oreilly.com`, see above) but refresh status unconfirmed; (5) what access Sentify needs — they're requesting full admin on both DC and Cloud instances (access-model decision, not yet made). Q1/Q2/Q4 need ITOps input; Q5 needs a decision from Dean Roman/Shawn Storc.
* **Access Request (2026-07-28):** [AM-2484](https://intranet.oreilly.com/jira/browse/AM-2484) filed by Shawn Storc, assigned to Jay — Sentify needs Steve Brannon & Casey Stanfield on Data Center System Administrator + Cloud Organization Administrator, and Bhumika Talsania on Jira/Confluence (DC) access for delivery plans/KB collateral. Jay expects this will require setting up **Okta accounts** for the consultants. Due in the next 1-2 days (kickoff is 8/3). See `tickler.md` #22.

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

### 💡 Related ask (2026-07-22): direct LDAP/AD queries from Jay's Mac
Jay wants to run LDAP queries against AD directly from his (non-domain-joined) Mac over the O'Reilly VPN, to pull user/group lists more easily when working with Claude, as a stopgap while the Okta MCP idea above is pending approval. Same underlying need as that idea (live directory data on demand) but via a different path (raw LDAP bind vs. Okta API).

**Reachability confirmed 2026-07-22.** AD domain is **SEB01.COM** (found via a "Change Directory Server" screenshot from AD Sites and Services). Five DCs: `gcpdc1`/`gcpdc2.SEB01.COM` (site GCP, 10.200.100.10 / 10.200.144.3), `ocidc1.SEB01.COM` (site OCI, 10.220.0.12), `sebdc1`/`sebdc2.SEB01.COM` (site Sebastopol, 172.24.1.13 / .14). From Jay's Mac over VPN (utun4, corp DNS `10.200.103.238`):
- DNS SRV discovery works: `_ldap._tcp.dc._msdcs.SEB01.COM` resolves all 5 DCs.
- All 5 DCs reachable on both port 389 (LDAP) and 636 (LDAPS) via `nc -z`.
- Anonymous LDAP bind to `gcpdc2.SEB01.COM` succeeded and returned RootDSE (`defaultNamingContext: DC=SEB01,DC=COM`) — confirms the LDAP protocol itself responds, not just the TCP port.
- macOS ships `/usr/bin/ldapsearch` (OpenLDAP 2.4.28, Apple build) already — no `brew install` needed.

**Not yet tested:** authenticated bind for actual user/group object queries (anonymous almost certainly won't expose those — real query needs a bind DN + password). Next step is a read-only AD service account/credentials for Jay to bind with; store in 1Password per the existing `op` pattern ([op-cli-env-secrets-guide.md](../notes/op-cli-env-secrets-guide.md)) rather than plaintext. Prefer LDAPS (636) over 389 once a real credential is in play. Still worth comparing against the Okta MCP path once that's approved, rather than maintaining both long-term.

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
* **Description:** GDPR PRIV data-deletion requests need to check/anonymize users in **On24** (webinar/live-events platform) and **OLT** (Online Training Slack workspace). Current automation ([gdpr-priv-automation](https://github.com/oreillymedia/gdpr-priv-automation) repo) only checks whether the user still exists and updates the linked Jira ticket — it does not actually anonymize anything, and On24/OLT are not among the repo's automated closers (handled manually today, e.g. [SOL-101033](https://intranet.oreilly.com/jira/browse/SOL-101033)). **Confirmed 2026-07-15 by reading the repo directly:** the only automated ticket closers are `exactTarget.py` (ExactTarget) and `conf-mysql.py` (CONF/MySQL) — **SFDC is not a source system anywhere in this repo**, so SFDC PRIV tickets are also closed manually today, not automatically. Separately, [DAP-4584](https://intranet.oreilly.com/jira/browse/DAP-4584) (status: To Do as of 2026-07-15) is unrelated DB-layer work to make the `staging.sfdc_account` warehouse load actually anonymize PII — it does not close Jira tickets and hasn't shipped; it blocks Phase B work like [DAP-2487](https://intranet.oreilly.com/jira/browse/DAP-2487).
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
* **Design idea raised 2026-07-21:** Jay wants the daily agenda restructured to be more visible/structured from the outset — group each day's items into status sections (e.g. "Pending Approval"), and under each ticket show (1) a plain-language summary of what's actually being requested and (2) an AI-inferred analysis pulling in related Jira tickets/other resources for context, surfaced directly on the Live page agenda item rather than discovered live in the meeting. Motivating example: [AM-2464](https://intranet.oreilly.com/jira/browse/AM-2464) (Yi Ma requesting BigQuery access to `data-dev-378016`, assigned to Jay) — description gives zero business justification, no linked ticket, and the project is ITOPS-owned per [gcp-project-ownership.md](../notes/gcp-project-ownership.md) (should route through ITOPS/Alain Mbuku via `itops-infra-tf`, not be granted ad hoc) — exactly the kind of vague/questionable request AI context should flag before it hits the meeting.
* **Idea (raised 2026-07-23, not started):** have AI analyze the Claude Enterprise Usage tab per-user data and surface easy efficiency recommendations for each person — Jay's example: high chat-message count but low distinct-conversation count could mean someone should split work into more separate conversations for token efficiency (long single threads re-send full context each turn). Other angles worth exploring once picked up: high Claude Code session count with zero commits/PRs (maybe not landing work), heavy chat use with zero project/artifact usage (missing a feature that'd save them tokens), department-level outliers vs. peers. No action yet — explicitly deferred by Jay.
* **Claude Enterprise Usage tab, added 2026-07-23** to the [AI Tools Usage and Adoption](https://docs.google.com/spreadsheets/d/18AjAEWT9N3032T3PdzJeP9cisxy0CJx-JaHL5fkg6QI) sheet (the one referenced above as the AI usage/adoption dashboard) — per-user 30-day activity + cost (24h/7d/30d, grouped by department, highest-cost first) plus an org-level daily adoption summary since 2026-01-01, sourced from Anthropic's Analytics API (`read:analytics` scoped key, separate from the Console/API cost-tracking key already used elsewhere in that repo). **Now auto-refreshes daily (9:15am)** via `~/src/ai-usage-tracker/export_claude_enterprise_usage.py` (new script, added to the same crontab as the existing 5-minute Console/API export jobs, but on its own daily schedule since Analytics data only updates ~once/day). Full technical writeup on the [Claude Enterprise](https://oreillymedia.atlassian.net/wiki/spaces/~5bf49087da94df18b4d49b9b/pages/20086785/Claude+Enterprise) Confluence page.
* **New child section, created 2026-07-23:** [App Configurations](https://oreillymedia.atlassian.net/wiki/spaces/~5bf49087da94df18b4d49b9b/pages/20054018/App+Configurations) (id `20054018`, parent: Overview id `13107295`) — dedicated space for documenting access/role/limit config per internally-managed app (SSO/SCIM group mappings, custom roles, quota/spend limits, admin API access patterns). First child page: [Claude Enterprise](https://oreillymedia.atlassian.net/wiki/spaces/~5bf49087da94df18b4d49b9b/pages/20086785/Claude+Enterprise) (id `20086785`) — documents the SCIM/group-mapping setup, custom role built for Julie Baron, and the Admin/Compliance API key gotchas from the 2026-07-23 Enterprise config session. Add more apps here as they get documented.
* **Overlaps with:** the "Inbound Enrichment — Reporter Context" idea in [jira-ai-enrichment.md](../notes/jira-ai-enrichment.md) (reporter/team/manager lookup posted as restricted comment) — this 2026-07-21 idea extends that concept to also cover cross-ticket/resource analysis and to surface it on the Confluence agenda item itself, not just as a Jira comment.
* **Open questions:** how enrichment gets generated per item before each day's meeting (on-demand by Claude vs. a scheduled job), and how far "analysis" should go (e.g. asserting "no stated reason" or "wrong approval path" vs. just linking related tickets).
* **Details:** Confluence Cloud API access technicals for this space moved to [confluence-cloud-api-access.md](../notes/confluence-cloud-api-access.md).
* **Jira:** none

## 11. Managers Group Auto-Removal Automation
* **Status:** Proposed (details pending from Jay)
* **Description:** There's an existing automation that adds people to manager-related groups (e.g. `orm-all-managers@oreilly.com`, surfaced via the alias audit in [HD-37573](https://intranet.oreilly.com/jira/browse/HD-37573)) automatically based on changes to the latest roster. There is currently **no corresponding mechanism to remove people** from those groups when they're no longer managers — a gap Jay wants to close.
* **Current process:** Driven through a Google Sheet with formulas (mechanics TBD — Jay to provide details).
* **Next Step:** Jay to provide full details on the roster/sheet process and desired removal logic; then scope and open a Jira ticket to track the build.
* **Jira:** none yet — raised 2026-07-14, no ticket created yet

## 12. GitHub Service Account for Cloudbuild (oreillymedia-sales)
* **Status:** Open (unassigned, P2)
* **Description:** Dean Roman requested a GitHub service account in the `oreillymedia-sales` org, for use by Google Cloudbuild to pull and create builds. Builds run in the `orm-sales` GCP project.
* **Next Step:** Needs assignment/pickup.
* **Jira:** [HD-37642](https://intranet.oreilly.com/jira/browse/HD-37642)

## 13.5 Engineer Local Machine Backup Policy Discussion
* **Status:** Open (gathering backstory)
* **Description:** In `#team-eng-managers` (2026-07-16), **Greg Crowder** raised — on behalf of his team, ahead of the next MetaCon retro — that only `~/Desktop`, `~/Documents`, and `~/Downloads` are covered by the local machine backup policy. Most engineers keep code in differently-named dirs (`~/code`, `~/repos`), so uncommitted/unpushed work isn't backed up; this surfaced when a team member's laptop died. Questions raised: standardize a code-dir name for backup purposes? Move code dirs under `~/Documents`? Document the decision in devdocs?
* **Aaron Sumner's take:** believes backup software and git repos don't play nicely together, so the guidance has been to keep code out of backed-up directories — deferred to Jay to confirm. Also shared his own tool, [gh-clone-team-repos](https://github.com/ruralocity/gh-clone-team-repos), for bulk-recloning a GitHub team's repos after a machine loss.
* **Jay's backstory (recalled, not yet verified):** engineers previously asked to have their code dir *excluded* from backup because it was slowing them down — **Ben Kreeger** may have been involved. Jay plans to search prior Slack history to confirm before responding in the thread.
* **Next step:** Jay to search Slack for the earlier backup-exclusion discussion, then reply in `#team-eng-managers` with the historical context and a recommendation.
* **Jira:** none

## 14. Gemini Code Assist Code Review GitHub App Sunset
* **Status:** Active — **deadline 2026-07-17 (tomorrow)**
* **Description:** Google is sunsetting the consumer version of the Gemini Code Assist Code Review GitHub App. New org installations blocked since 2026-06-18; all code review activity officially ceases **2026-07-17**. 47 O'Reilly repos currently use this app (per Gary/Dean's GitHub check) — in use broadly across engineering, not just SRE.
* **Options being investigated (Slack thread w/ Google/Insight reps, as of 2026-07-16):**
  * Enterprise version of Gemini Code Assist on GitHub is a **separate/distinct product** from Gemini Code Assist Enterprise (per Google docs) — but in practice Devin found the Enterprise Cloud project console only lets you enable the whole Gemini Code Assist product, not just the Code Review piece. Contradiction not yet resolved.
  * Arvind Vijayanand (Google) confirmed code review features **can** be enabled via an Enterprise Cloud project, and said there's a plan for **standalone pricing just for code review** — timeline unconfirmed, Arvind following up with Product team.
  * Suggested path: set up **Developer Connect** to link GitHub to Google Cloud, following the Gemini Code Assist-specific setup doc (not the generic Developer Connect codelab) since the Code Review engine needs specific region routing/backend binding.
  * Google's broader recommendation is their new "Antigravity 2.0" multi-agent platform / Gemini Enterprise Agent licensing — separate conversation, not a fix for the immediate deadline.
* **Devin Cooley's read:** this is exiting SRE's realm and entering Solutions/Access Management territory (GCP + GitHub access management) — he flagged updating the existing Solutions ticket.
* **Risk:** Kevin Graves (2026-07-16) warned that once the tool disappears at the deadline, expect support requests org-wide asking "what happened to it."
* **Jira:** [HD-37632](https://intranet.oreilly.com/jira/browse/HD-37632) — Open, P2, assigned to **Michael Seneschal**.

## 13. Rapid7 MDR Environment Review
* **Status:** Active (recurring vendor review)
* **Description:** Recurring review meeting with Rapid7 (MDR/SOC vendor) covering agent coverage, legacy OS agents, event source ingestion health, and cloud logging visibility.
* **Latest meeting:** 2026-07-14. Agent count dropped to 769 (needs verification against decommissions). Mac full disk access error on Brad's device; Linux audit compatibility errors on 11 devices. Core event sources (DNS/DHCP/firewall) have ingestion errors — Alon following up. Google Cloud logging setup delayed pending Brad's availability; critical given O'Reilly's GCP footprint.
* **Action items:** Elton to confirm Sophos event source/licensing status; team to review agent management page for stale/missing agents; Brad's Mac error + Linux team's audit errors to be resolved; Alon to clean up non-ingesting event sources; follow up with Brad on GCP logging + SCC event source config; regular review of custom detection tuning with SOC.
* **Full notes:** [rapid7-mdr-environment-review-2026-07-14.md](../notes/rapid7-mdr-environment-review-2026-07-14.md)
* **Jira:** none

## 16. "Superanswers" Internal Docs Platform (markdown-comments)
* **Status:** Active — multiple open threads across projects
* **Description:** Internal docs platform built by **Doug Hogan** (Engineering Director) on GitHub Pages, backed by the GitHub repo `oreillymedia/markdown-comments`. Branded externally as **"O'Reilly Superanswers."** Repos are Internal-visibility, so consumer doc sites currently live at random GitHub-generated hostnames (e.g. `legendary-adventure-zgemyyk.pages.github.io`, `effective-adventure-qj272gj.pages.github.io`) rather than clean URLs.
* **Architecture pieces:**
  * **[PE-5447](https://intranet.oreilly.com/jira/browse/PE-5447)** (Closed) — Cloud Function + gateway for internal GitHub App auth, gating access to the Internal-visibility Pages sites.
  * **[PE-5544](https://intranet.oreilly.com/jira/browse/PE-5544)** (Closed, 2026-07-17) — Vault read grant for the `markdown-comments-slack` service account — backend for the Slack integration.
  * **[ITOPS-44479](https://intranet.oreilly.com/jira/browse/ITOPS-44479)** (Closed) — DNS TXT domain verification + wildcard CNAME (`*.internal-docs.oreilly.com` → `oreillymedia.github.io.`), so future doc sites get branded subdomains with zero added DNS work — just commit a `CNAME` file and configure Pages.
  * **[SE-1111](https://intranet.oreilly.com/jira/browse/SE-1111)** (To Do, unassigned) + **[ITOPS-45274](https://intranet.oreilly.com/jira/browse/ITOPS-45274)** (In Progress, Dean Roman) — applying a specific branded subdomain under the wildcard above, likely to move a site off its random `pages.github.io` hostname.
  * **[AM-2460](https://intranet.oreilly.com/jira/browse/AM-2460)** (Pending Approval, assigned Michael Seneschal) — Slack app "O'Reilly Superanswers" install request, so links to the doc sites unfurl properly in Slack instead of showing a GitHub login page. Currently scoped to unfurl the two random-hostname URLs above (not yet the branded subdomains).
* **Next Step:** No action needed from Jay currently — AM-2460 awaiting approval in the normal queue. Watch for SE-1111/ITOPS-45274 to land, which may prompt an update to AM-2460's scoped URLs once the branded subdomain goes live.
* **Jira:** AM-2460, SE-1111, ITOPS-45274, ITOPS-44479, PE-5447, PE-5544

## 15. AI Vibe Coding at O'Reilly — Hackathon Prep
* **Status:** Active — plan agreed, scoping details next (as of 2026-07-16)
* **Description:** Dean Roman is writing a guide to "AI Vibe Coding at O'Reilly" to direct employees during an upcoming hackathon (distinct from the earlier hackathon referenced in `knowledge/slack-digests/2026-06-26-ai-chat.md`; date/scope of this one still TBD). Dean's message to Jay (Slack, 2026-07-16 4:17pm) laid out what he expects Help Desk to handle beforehand.
* **Help Desk asks from Dean:**
  * **Training / basic how-tos** on the tools people will use: GitHub, Homebrew, Claude Desktop, Codex, Antigravity, etc.
  * **Pre-rollout** (get installed/configured ahead of time, ideally company machines already have these before the hackathon starts) — proposed list:
    * Homebrew
    * Claude (Desktop/Code)
    * Codex
    * Antigravity
    * Node, Go, Python
    * GitHub client (`gh` CLI) and GitHub Desktop
    * **1Password CLI (`op`)** — where not already installed. See [op-cli-env-secrets-guide.md](../notes/op-cli-env-secrets-guide.md), written for this same "vibe coding" push, for the secrets-handling piece.
    * Any Claude/Codex/Antigravity skills or config files worth pre-loading
* **Decisions (Slack, 2026-07-16 4:26–4:29pm):**
  * Jay confirmed most/all of the pre-rollout list can likely be done via **Iru** (MDM tool Elton Lee manages — see `knowledge/key_people.md`).
  * Dean and Jay agreed to hold **office hours leading up to the hackathon** so people still having trouble can get help.
* **Next step:** Jay to figure out what's feasible via Iru for bulk pre-rollout (per-app packaging/scripting) and coordinate with Elton Lee; scope/schedule the pre-hackathon office hours. Watch for Dean's guide draft and the hackathon date.
* **Jira:** none yet

## 17. AI-Assisted Access Management Request Process (idea)
* **Status:** Idea — meeting being scheduled with Sanjay Khona (Engineering)
* **Description:** Jay's proposal for an agentic pipeline for IT access requests: intake a request filed as a **SOL** ticket → gather relevant context from other Jira tickets and other systems → ask the requestor follow-up questions → produce a normalized **AM** ticket with the correct approver assigned. Inspired by **Sanjay Khona's "Agent Zero"** project (see `key_people.md`) — different subject matter (engineering ticket resolution vs. access requests), similar architectural shape (intake → research/context-gathering → structured output, human-gated).
* **Related idea:** extends the "Inbound Enrichment — Reporter Context" concept in [jira-ai-enrichment.md](../notes/jira-ai-enrichment.md) (reporter/department/manager lookup posted as a restricted comment) — this goes further by normalizing the ticket itself and assigning the correct approver, not just annotating context.

## 18. pb-jira-backlinker — Hosting Request (SOL-107192)
* **Status:** Open, P2, unassigned — added to IT Standup for discussion (2026-07-22). **Runtime decided 2026-07-22: Dean Roman (VP, Information Systems) directed ITOPS to set this up on Cloud Run**, overriding the ADR's Lean Chassis recommendation below — Jay to send the ITOPS hosting request accordingly.
* **Description:** **Shawn Storc** filed [SOL-107192](https://intranet.oreilly.com/jira/browse/SOL-107192) asking whether a small ProductBoard↔Jira reconciler tool (`pb-jira-backlinker`) should be a **chassis app or Cloud Run app**, ahead of asking ITOPS to set up hosting. No code exists yet — repo `oreillymedia-product/pb-jira-backlinker` (private) currently holds only `README.md` and `SPEC.md` (status: design/RFC).
* **What the tool does:** scheduled reconciler (no webhook, no public HTTP endpoint) that reads ProductBoard↔Jira connections via the ProductBoard v2 API and writes/removes an idempotent Jira remote issue link (`productboard entry`) pointing back to the PB entity. Runs ~every 10 min; state stored in one Postgres row per link.
* **⚠️ Contradiction to flag before this goes to ITOPS:** the ticket's own attached decision brief (`0001-decision-brief.md`, summarizing **open PR #2**, [ADR-0001](https://github.com/oreillymedia-product/pb-jira-backlinker/pull/2)) recommends the **opposite of GCP/Cloud Run** — it argues for **Lean Chassis** (Postgres + CronJob feature, paved-road Jenkins+ArgoCD deploy, Vault secrets, auto Cortex/monitoring) because Cloud Run on this project has zero DevDocs coverage, no reusable Terraform module, and manual Cortex/monitoring setup. PR #2 is still **open, seeking team input** — the runtime question is not yet settled. Sending ITOPS a request to "set up hosting on GCP" would preempt that unresolved decision and go against the recommendation in the very brief attached to the ticket.
* **Spec inconsistency:** `SPEC.md` §5 says the tool talks to **Jira Cloud (REST v3)**, but the attached decision brief says **"Jira DC."** O'Reilly's Jira is still Data Center in production (see item #4, Atlassian Cloud Migration — Discovery phase, not yet cut over), so the spec's Jira Cloud API assumption looks stale/wrong and should be corrected before build.
* **Audience / auth (per SPEC.md):** no end-user-facing audience — it's an unattended backend job; the only "consumers" are people later viewing a Jira issue that gained a ProductBoard back-link. No inbound authentication needed (no public endpoint, webhook explicitly deferred to phase 2 per SPEC §9) — it only makes outbound authenticated calls to ProductBoard (API token) and Jira (account email + API token), both stored in Vault.
* **Since the Cloud Run decision came from Dean, not from resolving PR #2 on the merits:** worth flagging to ITOPS at request time (not blocking, just so they're not surprised) — the brief's evidence was that Cloud Run has **no paved road here**: no DevDocs coverage, no reusable Terraform module, deploy is manual `docker build/push`/`infractl` (not Argo), and Cortex/monitoring setup is manual (no auto-import). ITOPS should expect to hand-roll those pieces rather than getting them for free the way a chassis app would.
* **Next step:** Jay to send the ITOPS hosting request for Cloud Run per Dean's direction. Still worth fixing the Jira Cloud vs. DC spec discrepancy in `SPEC.md` before build regardless of runtime — that's independent of the Chassis/Cloud Run choice.
* **Jira:** [SOL-107192](https://intranet.oreilly.com/jira/browse/SOL-107192)
* **Meeting:** Dean Roman asked Jay to set up time with Sanjay to discuss his design/process and what's reusable. Sanjay agreed (Slack, 2026-07-22) to any Monday–Thursday 3:30pm ET slot. **Next step:** Jay to ask Dean tomorrow (2026-07-23) for the best specific day, then send Sanjay the calendar invite. See tickler item #21.
* **Jira:** none yet

## 19. Claude Connector to Google Drive — Blocked on PII ([TOOLS-369](https://intranet.oreilly.com/jira/browse/TOOLS-369))
* **Status:** Open, "To Do" — not yet approved (as of 2026-07-24; last Jira update 2026-04-07)
* **Description:** Margaret Shelman (manager Desi Gonzalez) requested the Claude Google Drive connector be enabled org-wide so Claude can search/read/analyze files in Drive. Ticket's own "Data Shared With Tool Vendor" field is "Unsure - to be determined."
* **Jay's blocker (per conversation with Jennie, 2026-07-24):** potentially significant customer PII is scattered across O'Reilly's Google Drive with no current way to separate/scope it out, so there's no clean way to grant Claude Drive access without exposing that PII to the vendor. This is the reason it hasn't been approved.
* **Jira:** [TOOLS-369](https://intranet.oreilly.com/jira/browse/TOOLS-369)
