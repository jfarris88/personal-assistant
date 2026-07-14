# Inbound — Drop Zone

Raw capture zone. Drop anything here without ceremony: exports, screenshots (`inbound/screenshots/`), docs, rosters. Filenames don't need to be clean — that happens at processing time.

**This folder is gitignored — nothing here is version-controlled.** It routinely contains sensitive employee/personnel data (MFA/compliance exports, rosters), so keep it that way; never move raw files here into a tracked location without triaging first.

## Lifecycle

1. **Drop** — file lands in `inbound/` (or `inbound/screenshots/` for images).
2. **Process** — run `skills/process_inbound.md` (files) or `skills/process_screenshot.md` (images): extract what's useful, route it into the right tracked file (`knowledge/`, `notes/`, `tickler.md`, `daily/`), present to Jay for approval.
3. **Disposition** — once approved:
   - **Keep** → move to `reference/` with a clean filename, index in `knowledge/reference-documents.md`.
   - **Delete** → remove from `inbound/` once its content is fully absorbed elsewhere.
   - Nothing should sit in `inbound/` indefinitely as "still pending" without a plan.

## Current backlog (as of 2026-07-14)

The following files are still awaiting triage with Jay (keep/archive/delete + what it fed) — not yet processed as part of this reorganization:

- `AI Strategy for Sales teams.xlsx`, `AI Strategy for Sales teams (1).xlsx`, `AI Strategy for Sales teams - reviewed.xlsx` — likely duplicates, needs Jay to confirm which is canonical
- `Claude members 6-18members-...csv`
- `Create a read-only database user using create_ro_database_user.sh.md` — already indexed in `knowledge/reference-documents.md`, flagged for Confluence relocation; candidate to move to `reference/` and delete from here
- `Latest Roster - Roster.csv`
- `Okta-mfa-users_...csv`
- `Request for member access Editors and Production...csv`
- `Salesforce Users okta group.csv`
- `Salesforce_Users_Missing_Okta_Verify.xlsx` — tracked in `tickler.md` item on Salesforce Okta Verify follow-up, keep until that's sent
- `Salesforce_Users_Missing_PhishingResistant_MFA.xlsx`
- `Smartsheet Seat type report...csv`
- `exported_users_20260617_172629.csv`

Process these with `skills/process_inbound.md` in a session with Jay in the loop for keep/archive/delete calls.
