# Confluence Cloud Migration — Candidate Docs

Running list of legacy documentation (Confluence Data Center pages, Google Docs, etc.) flagged for relocation into the new IT/Helpdesk Confluence Cloud space once it exists. See [Atlassian Cloud Migration](current_projects.md) (current_projects.md, section 5) for the migration project itself — this file just tracks *what* should move over.

| Flagged | Title | Current Location | Why It Matters | Notes |
|---|---|---|---|---|
| 2026-06-29 | Create a read-only database user using create_ro_database_user.sh | [Google Doc](https://docs.google.com/document/d/1XgwrQGx0ucy1gEMSzPcFXjAl-CgNQNweh08TycWZQVA/edit?tab=t.0) | Runbook for provisioning read-only DB users, actively used by engineering | See [reference-documents.md](../knowledge/reference-documents.md) |
| 2026-07-09 | [Adding SSH keys for Linux system authentication](https://intranet.oreilly.com/confluence/pages/viewpage.action?pageId=79135041) | Confluence Data Center, space IS (pageId 79135041) | Core reference for adding an SSH public key to a user's Okta profile — needed for CDN origin access, Linux system auth, and any key-based auth workflow. High-traffic/high-importance doc. | See [cdn-access-process.md](cdn-access-process.md) |

## Capture Convention

When a doc gets flagged as important-but-stuck-in-a-legacy-location, add a row here with the date, title/link, current location, and why it matters. Once actually migrated, move the row to a "Migrated" section (create one below) with the new Cloud link.
