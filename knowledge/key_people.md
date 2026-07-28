# Key People

*This file helps the assistant understand who is being referred to in brain dumps. Update as you learn more about colleagues.*

## My Info
* **Jay Farris** (`jfarris`, legal name Nathan Jay Farris): Manager, Helpdesk — IT Operations at O'Reilly Media. Reports to Dean Roman. Manages AI tooling rollout (Claude Enterprise), MCP infrastructure, helpdesk, access management, and hardware provisioning.

## My Direct Reports
* **Elton Lee** (`elee`): Sr. Desktop Support Technician — MDM (Iru), app deployment, Windows/Mac management
* **Michael Seneschal** (`mseneschal`): Sr. Desktop Support Technician
* **Jemrey Paraluman** (`jparaluman`): Desktop Support Technician — based in Philippines

## My Recurring Meetings
* **Helpdesk team daily standup**: 12:30pm PT, Monday–Thursday

## Management & Leadership
* **Dean Roman** (`droman`): VP, Information Systems — Jay's manager; sent company-wide ChatGPT rollout email
* **Julie Baron** (`jbaron`): President — **her requests always take priority over other work.** Requested Claude admin/reporting dashboard access again after it was changed when billing moved to Jay's team, plus org-wide ChatGPT admin/reporting view (AM-2431, filed 2026-07-10)
* **Baaqir Yusuf**: Anthropic contact — Dean Roman's point of contact for questions about what changes on O'Reilly's Claude account once the Enterprise Agreement is signed (seat management options, etc.). See AI Native Initiative in `current_projects.md`.

## Known Colleagues
* **Mike Loukides** (`mikel`): VP, Content Strategy — frequent helpdesk submitter; recurring Sophos/WiFi issues on travel
* **Thomas McIntosh** (`tmcintosh`): Salesforce Administrator — involved in Salesforce phishing-resistant MFA enforcement project; requested Gong/Amplemarket integration (HD-37515)
* **Keith Swafford**: Role unknown — being pulled into HD-37515 Gong/Amplemarket discussion meeting with Jay, Thomas McIntosh, and Dean
* **Cynthia Saalfeld** (`csaalfeld`): Salesforce Sales Operations Specialist — reported HD-37479 (SSO/MFA compliance)
* **Mike Boutté** (`mboutte`): Director, Product Design
* **Shawn Storc** (`sstorc`): Project Manager — PE (Platform Engineering) team PM; also involved in Atlassian cloud migration. Point of contact for routing requests to PE.
* **Brad Frank** (`bfrank`): Platform Engineering — PE team member; member of the `#atlassian-cloud-migration` Slack channel. **Note (2026-07-17): does not own the DC Atlassian instance** — that's ITOps (see note below), not PE.
* **Justin Breninger** (`jbreninger`): **Transferred out of Platform Engineering as of 2026-07-23** — no longer the go-to for GCP infra/SA/IAM tasks; previously handled service account creation/deletion in `platform-prod-399563` and `platform-dev-788014`. Need to find his replacement/successor for this work (surfaced when he was assumed to be the assignee for the AM-2475 gcloud reauth fix — he is not, since he's moved on).
* **David Carroll** (`dcarroll`): Platform Engineering — handles RabbitMQ, Pub/Sub, and other platform infra requests
* **David Buckley** (`dbuckley`): ITOPS — handles GCP Terraform work for DAP/data-* projects (Cloud Run, Pub/Sub infrastructure)

* **Angela Rufino** (`arufino`): Development Editor, Prod Dev Content — asked whether WIP manuscripts are considered non-restricted content under the ChatGPT usage policy
* **Heather Polzin** (`hpolzin`): Director, Global Sales Development — Nooks admin/owner, point of contact for Nooks licensing and SSO setup, coordinating with Nooks team on free admin seat for IT procurement account. Filed [HD-37652](https://intranet.oreilly.com/jira/browse/HD-37652) requesting the Salesforce Slackbot integration be turned on for team pipeline/performance reporting.
* **Jamey Harvey**: Finance/billing contact — Nooks invoices should be routed to him (currently not receiving them); also the person who assigns hardware tasks (requires a Jira ticket with full details before assigning)
* **Theodore Moser** (`tmoser`): Project Manager — building signal-iq internal intelligence dashboard for leadership; requested service account API keys (AM-2301) for Jira, Anthropic, Productboard, Cortex, Smartsheet, Workfront, Amplitude; uses Anthropic to process/analyze aggregated project data
* **Vicky Dutkiewicz**: Contact for G&A and Operations AI use cases sheet
* **Cali Bail**: Involved in AI Native initiative / ChatGPT usage policy (WIP manuscripts question); contact for G&A and Operations AI use cases sheet
* **Becki Valente**: Contact for G&A and Operations AI use cases sheet; has a pending Claude Standard seat request (AM-2257)

* **Nick Adams** (`nadams`): Tools/Engineering team — manages Atlas group permissions on the backend; the person who adds users to the appropriate Atlas group after self-signup. See [Atlas Access Process](../notes/atlas-access-process.md).
* **Scott Moschella**: Engineer working with the feedback-service / Interactivity team — grants Django admin permissions for feedback-service (QA & Prod) when a user logs in but sees a blank dashboard. Point of contact in Slack `#vs-micro-feedback-and-diagnostic-collab`. See [Chassis Service Admin Access Process](../notes/chassis-service-admin-access-process.md).
* **Desi Gonzalez**: ID (Instructional Design) team — driving the feedback-service microfeedback UAT effort (SOL-106969); requested QA/Prod admin access for the ID team.
* **Charlotte Ames**: ID team — involved in the microfeedback UAT decision-making with Desi Gonzalez.
* **Illana Stanley**: Doing e2e QA testing of quiz microfeedback for the feedback-service rollout.
* **Aaron Sumner** (`asumner`): Engineering — involved in Atlas documentation; was assigned to document the Atlas account expiration fix (OREILLYSTAFF vs OREILLYUK registration codes). Also raised the local machine backup policy question in `#team-eng-managers` (2026-07-16) on behalf of his team; built [gh-clone-team-repos](https://github.com/ruralocity/gh-clone-team-repos) (bulk-reclone a GitHub team's repos) after his own laptop died last year.
* **Greg Crowder**: Engineering manager — started the `#team-eng-managers` thread on local machine backup policy (2026-07-16), prompted by a team member's laptop dying with uncommitted code loss; wants to raise it before the next MetaCon retro.
* **Ben Kreeger**: Engineering — possibly involved in a past decision/discussion about excluding engineers' code directories from machine backups (reason: it was slowing them down). Jay to confirm by searching prior Slack history.
* **Rachel James**: U&A (User & Access) — driving the On24 GDPR anonymization automation effort with Flex 4; reaching out to Jay/Help Desk/Live Events to hook up the Groot → On24 automation. See [On24 Anonymization Automation](../notes/on24-anonymization-automation.md).
* **Susan Strom**: Added as a courtesy invite (per Shawn Storc) to the On24 anonymization automation discussion.
* **Patrick Devine**: Assignee/closer on manual On24/OLT GDPR anonymization tickets (e.g. SOL-101033) before the Flex 4 automation existed.
* **Michelle Chen**: Remote employee (China) awaiting a Windows laptop shipment — profile setup blocked on her providing her password so it can be imaged before shipping. See tickler item #14.
* **Yasmina Greco** (`ygreco`): Working with Lindsay on a live cohort/event program (same effort as [HD-37306](https://intranet.oreilly.com/jira/browse/HD-37306)); requesting a Zoom Webinar Plus subscription for branded, recurring-meeting registration. See tickler item #15.
* **Lindsay**: Working with Yasmina Greco on the live cohort event / Zoom Webinar Plus request. Last name not yet captured — confirm on next contact.
* **Mace Bergmann** (`mbergmann`): Business Development — frequent helpdesk submitter (Okta/app access, RingCentral, laptop performance); had a BitLocker recovery on 2026-07-14 (Jay supplied the recovery key via Slack), device rebooted through a Secured Dell SafeBIOS prompt and came back up fine. No Jira ticket filed for this event as of 2026-07-14.
* **Alon**: Rapid7 MDR contact (SOC/vendor side) — following up on non-ingesting event sources (DNS/DHCP/firewall). Last name not yet captured. See [Rapid7 MDR Environment Review](../notes/rapid7-mdr-environment-review-2026-07-14.md).
* **Sanjay Khona**: Engineering — building **Agent Zero**, an internal agentic ticket-resolution system (three projects: slack-to-jira-integration, research-agent, coding-agent). Meeting with Jay (and Dean, at Dean's request) to discuss his design/process and what's reusable for Jay's AI-assisted access management request idea. See `current_projects.md` #17.

*(Add more as they come up)*

## Department Heads — AI Tool Review (June 2026)
As part of the AI Native initiative, Jay is reaching out to department heads to have them review which employees on their teams should be assigned Claude. Track these conversations as they happen and note which departments have responded / been assigned seats.

## Notes on Inference
* If a name appears in a brain dump without context, check recent Jira tickets in `HD`, `AM`, and `SOL` — they often reference the person's name and the nature of the request.
* When Jay says "Michael" without a last name, that's Michael Seneschal (`mseneschal`) on his team.
