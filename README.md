# Personal Assistant Project (Project Agy)

Jay Farris's personal executive-assistant system at O'Reilly Media. This folder is the **shared source of truth** for every AI system working with Jay (Claude, Gemini, ChatGPT) — not just session memory. If it's worth remembering, it belongs in a file here.

## Start here (fresh AI session)

1. Read `SYSTEM_PROMPT.md` — full behavioral contract: workflow, daily briefing process, tickler handling, standing rules (NHT off-limits, never touch Jira without say-so, "FYI" = save it).
2. Read `knowledge/working-style.md` — how Jay works and how to work with him.
3. Read `knowledge/current_projects.md` and `knowledge/key_people.md` — current context.
4. Read `tickler.md` — anything due to be surfaced today.

Claude also reads `CLAUDE.md`; Gemini and ChatGPT/Codex have thin pointer files (`GEMINI.md`, `AGENTS.md`) that redirect here and to `SYSTEM_PROMPT.md`.

## Folder map — where things go

| Folder / file | What it's for |
|---|---|
| `SYSTEM_PROMPT.md` | Full assistant behavior contract — read by every AI, every session |
| `CLAUDE.md` / `AGENTS.md` / `GEMINI.md` | Per-AI entry points, all pointing back to `SYSTEM_PROMPT.md` |
| `knowledge/current_projects.md` | Active projects and initiatives — status, people, next step, Jira links |
| `knowledge/key_people.md` | Colleagues — roles, teams, recurring meetings |
| `knowledge/working-style.md` | How Jay works and how the assistant should behave with him |
| `knowledge/reference-documents.md` | Index of saved reference docs (emails, runbooks, repos, tools/systems) |
| `knowledge/research-links.md` | Links to read/investigate later |
| `knowledge/slack-digests/` | Saved weekly Slack-AI digest summaries |
| `daily/YYYY-MM-DD.md` | Daily briefing — tickler matches, action items, carry-over |
| `tickler.md` | Proactive reminders, keyed to a date or conversation trigger |
| `notes/kebab-case-title.md` | One-off reference notes and deep-dive detail (linked from `current_projects.md` or elsewhere) |
| `jira-drafts/` | Staged/drafted Jira tickets before or after pushing to Jira |
| `handoff/` | End-of-session handoff notes for another AI or a future session |
| `templates/` | Format templates for Jira tickets, Confluence pages, daily notes, notes, handoffs |
| `reference/` | Archived source documents (email PDFs, screenshots, etc.), tracked in git |
| `inbound/` | Drop zone for raw files (screenshots, exports, docs) awaiting triage — **gitignored, not tracked** (contains sensitive employee data); see `inbound/README.md` for the lifecycle |
| `skills/` | Reusable processing instructions (brain dump, Atlassian workflow, screenshot/inbound processing) |

## Version control

This is a **local-only git repo** — no remote. `inbound/` is gitignored because it can contain sensitive employee/personnel data (MFA rosters, offboarding exports); nothing in it should ever end up in git history. `reference/` and everything else is tracked normally.

## Multi-AI conventions

- Save new context immediately using the routing table above — don't rely on any one AI's session memory.
- **"FYI" means save it.** Any process, preference, person, or tool Jay mentions gets written down.
- **Never modify Jira tickets** without Jay's explicit say-so in that session.
- **Never access the `NHT` Jira project** — sensitive employee offboarding data, not approved for AI access.
