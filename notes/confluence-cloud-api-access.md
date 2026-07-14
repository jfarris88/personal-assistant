# Confluence Cloud API Access — Jay's Personal Space

**Tags:** #note
**Related:** [current_projects.md — Helpdesk Standup Notes](../knowledge/current_projects.md), [current_projects.md — Atlassian Cloud Migration](../knowledge/current_projects.md)

## Overview
Technical notes on calling the Confluence Cloud API for Jay's personal space (`oreillymedia.atlassian.net`), gathered while building the Helpdesk Standup Notes v2.0 pilot.

## Details
- Uses a **granular-scoped API token** — 1Password item `oibcpx6fliyowhyqfc3bvute4e`, "Confluence API token (under my account)", Employee vault.
- Must be called through the `api.atlassian.com/ex/confluence/{cloudId}` gateway — **not** the site domain directly.
- Use the **v2 REST API** (`/wiki/api/v2/...`).
- Classic combined scopes (`read:confluence-content.all` etc.) do **not** work for this — granular scopes only (`write:page:confluence`, `read:space:confluence`, `read:page:confluence`, etc.). Classic scopes can't create live pages at all (v2-only feature).
- **Live pages** use `atlas_doc_format` (ADF) as their body representation, not `storage`.
- Config lives in [.env](../.env) at the project root: `CONFLUENCE_URL`, `CONFLUENCE_CLOUD_ID`, `CONFLUENCE_API_BASE`, `CONFLUENCE_EMAIL`, `CONFLUENCE_API_TOKEN` (as an `op://` reference), `CONFLUENCE_PERSONAL_SPACE_ID`/`KEY` — resolve secrets at runtime with `op run --env-file=.env -- <command>`.

## Action Items
*(none — reference material)*

## References
- Jay's personal Cloud space: "Jay Farris", spaceId `13107203` on `oreillymedia.atlassian.net`
- [current_projects.md](../knowledge/current_projects.md)
