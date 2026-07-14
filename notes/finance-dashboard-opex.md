# Finance Dashboard — "OpEx Command Center" — Technical Detail

**Tags:** #note
**Related:** [current_projects.md — Finance Dashboard](../knowledge/current_projects.md)

## Overview
Stack, hosting, and data-access detail for the internal Finance reporting dashboard built by Michael Trice (Senior Financial Analyst), replacing the team's manual Google Sheets reporting process. Summary/status lives in `current_projects.md`; this file holds the depth.

## Details
- **Stack:** React/Vite frontend (port 3000), Node/Express backend proxy (port 3001); data source is Google Sheets via a GCP service account (project `orm-finance-prod`), with plans to write forecast data back to Sheets under individual-user OAuth attribution.
- **Hosting/Auth:** Okta SSO (SPA app, Authorization Code + PKCE), modeled after the Data Science team's setup (`data-science-apps.corp.oreilly.com`). Target URL: `finance-apps.corp.oreilly.com`. Okta groups: `finance-admin`, `finance-Sales`, `finance-Marketing`, `finance-Content_Services`, `finance-G_A`, `finance-Tech_Engineering`, `finance-Product`, `finance-Legal_HR`.
- **Repo:** `oreillymedia/finance-platform` (private GitHub repo, Michael Trice primary owner, Data Science team has write access).
- **Data access:** BigQuery read access to `data-prod-378016` distribution layer (usage, Salesforce, Zuora, finance/EBS data) — granted to Michael Trice first, then mirrored to other Finance/FP&A team members.
- **Jira trail:** [ITOPS-43901](https://intranet.oreilly.com/jira/browse/ITOPS-43901) (SSO+hosting request) → [HD-37037](https://intranet.oreilly.com/jira/browse/HD-37037) (GitHub repo) → [HD-37428](https://intranet.oreilly.com/jira/browse/HD-37428) (Okta app provisioning) → [HD-37462](https://intranet.oreilly.com/jira/browse/HD-37462)/[HD-37572](https://intranet.oreilly.com/jira/browse/HD-37572) (Okta group user adds) → [DAP-4268](https://intranet.oreilly.com/jira/browse/DAP-4268)/[HD-37491](https://intranet.oreilly.com/jira/browse/HD-37491) (BigQuery access) → [AM-2412](https://intranet.oreilly.com/jira/browse/AM-2412) (Datadog API key, current as of 2026-07-08).

## Action Items
*(none — reference material)*

## References
- [current_projects.md](../knowledge/current_projects.md)
