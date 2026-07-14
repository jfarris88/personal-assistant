# On24 Anonymization Automation

## Background
GDPR "right to be forgotten" (PRIV) requests need to check/anonymize a user's data across downstream systems, including **On24** (webinar/live-events platform) and **OLT** (O'Reilly Online Training Slack workspace). Current state (as of 2026-07-10): the automation only **checks whether the user still exists** in On24/OLT and updates the linked Jira ticket accordingly (closes it if the user is gone). It does **not** perform any actual anonymization.

**Flex 4** (external team) has separately built automation to actually anonymize users in On24, hooking into automation U&A (User & Access team, presumably) already has in place. This work was on hold for a while and has recently been picked back up — now also covers **external event attendees (non-Groot users)**, which was the main gap previously. Rachel James (U&A) reached out to discuss hooking this up to the GDPR process and what workflow changes it implies.

## Key artifacts
- **GitHub repo:** [oreillymedia/gdpr-priv-automation](https://github.com/oreillymedia/gdpr-priv-automation) — Python/shell automation for GDPR PRIV data-deletion requests. Flow: collector scripts (`pdbUsers.py`, `filemakerUsers.py`, `exactUsers.sh`) pull current user emails from PDB, FileMaker (GCS), and ExactTarget (SFTP) into a Google Sheet/CSV. Closer scripts (`exactTarget.py`, `conf-mysql.py`) search Jira for open service tickets, extract the requested email from the linked privacy ticket, check if it still exists in the source, and if gone, post an internal comment and transition the ticket to Done. `conf-mysql.py` is the more hardened template (timeouts, retry/backoff, dry-run env vars) vs. `exactTarget.py`. **On24/OLT are not among the automated closers in this repo** — those tickets appear to be closed manually today (see SOL-101033 below).
- **Google Doc:** "On24 Anonymization Rollout" (id `1BqBnYuVb81O_VgFd-adzJPbHD1nC5t2S3A12WSslsYc`, a copy Jay made from the original — the original doc is not shared with the `gcp-helpdesk-prod-dsadmin@helpdesk-prod-1.iam.gserviceaccount.com` service account, the copy is).
- **Example ticket:** [SOL-101033](https://intranet.oreilly.com/jira/browse/SOL-101033) — "B2C User Deletion - Anonymize user in On24/OLT". GDPR Support Request, Closed/Done. Reporter `jira-app-gdpr-priv` (bot), assignee Patrick Devine. Description covers the two current *manual* DSR paths: deactivating the user in the OLT Slack workspace, and emailing On24's privacy team (`dsr@on24.com`) or using On24's GDPR REST API to view/edit/"forget" a registrant's demographic data. Only comment (Patrick Devine, closing): *"User not found in ON24. User not found in Slack."* Created/closed 2025-06-15 → 2025-06-16. Label: `DeleteMe`.

## Slack thread — Rachel James (U&A) request (2026-07-10)
Rachel James asked whether Jay/Helpdesk is ready to hook up the Groot → On24 automation now that Flex 4 covers non-Groot attendees too, and proposed a meeting with reps from U&A, Help Desk, and Live Events. Shawn Storc suggested adding **Susan Strom** as a courtesy invite.

**Draft reply sent (approach agreed):** yes in principle, but nail down the integration contract first. Open questions to raise before/on the call:
- Trigger mechanism — API call, event/queue, or something U&A already exposes — and auth
- Sync vs. async — immediate success/fail, or poll/wait for confirmation before closing the ticket
- Identity mapping — Groot user ID ↔ On24 registrant ID resolving consistently for both the existence-check and the anonymization call, especially with non-Groot attendees now in the mix
- Failure/retry handling — what happens if anonymization fails partway, and who/what surfaces that instead of silently closing the ticket
- Test/pilot plan — run against a small known set before going live on real GDPR requests, given compliance stakes

## Status / Next step
Waiting on the meeting Rachel is scheduling (U&A, Help Desk, Live Events, + Susan Strom as courtesy) to define the integration contract above. See [current_projects.md](../knowledge/current_projects.md) for tracking.
