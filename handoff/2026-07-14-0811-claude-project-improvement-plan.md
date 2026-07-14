# Handoff: Project Agy Improvement Plan

- **Timestamp:** 2026-07-14 08:11 PT
- **LLM / system:** Claude Code (Fable 5) — plan authored for implementation by a **Sonnet session**
- **Topic:** Review of the personal-assistant project folder + phased improvement plan
- **Status:** Plan approved-pending — Jay asked for review + plan only; nothing implemented yet
- **Scope note for the implementing session:** Phases 1–2 are safe to implement without further approval once Jay says go. Phase 3 items marked ⚠️ need Jay's confirmation in-session. Phase 4 items need external approvals (Dean/policy) — do not action, only prep.

---

## Context: how Jay works (drives every design choice below)

Jay is disorganized by nature, keeps the big picture in his head, and needs **fast access to granular detail on demand**. The folder's job is: (1) frictionless capture — throw things in without ceremony; (2) reliable retrieval — chat pulls the right detail from folder + Jira/Confluence when asked. Multiple AIs (Claude, Gemini, ChatGPT) share this folder as the source of truth. Capture must be *easier than not capturing*, or it won't happen.

## Review: what's working (don't break these)

- **Structure is genuinely good**: `SYSTEM_PROMPT.md` + `skills/` + `templates/` + `knowledge/` + `tickler.md` + daily briefings with Ticket Watch. The 2026-07-13 daily note is the system at its best — proactive, aging-aware, specific.
- **Standing rules** (NHT off-limits, never touch Jira tickets without explicit say-so, "FYI means save it") are clear and load-bearing.
- **Knowledge files are rich and current** — `current_projects.md`, `key_people.md`, `reference-documents.md` capture real operational detail (Cortex API quirks, Confluence Cloud token scoping, Claude Team-vs-Enterprise findings).
- Conventions already exist for emails (`reference/emails/`), research links, slack digests, handoffs.

## Review: gaps found

1. **No version control.** Folder is not a git repo. One bad edit or overwrite by any of the three AIs and history is gone. `.gitignore` exists (covers `.env`) but git was never initialized.
2. **`inbound/` is an unindexed dumping ground.** 13 files, no README, no processed-vs-pending state, duplicate files (`AI Strategy for Sales teams.xlsx` + `(1)` + `- reviewed`), names with spaces, and **sensitive content** (MFA-gap user lists, rosters). Nothing says what's been acted on.
3. **No screenshot/capture pipeline** — Jay's stated #1 want. Nowhere to drop a Slack screenshot and have it become structured, searchable knowledge.
4. **`knowledge/current_projects.md` is bloating and disordered.** Section numbering runs 1,2,3,4,5,6,9,8,10,7. Section 7 (Standup Notes pilot) carries ~15 lines of API implementation detail that belongs in a `notes/` file. As it grows, retrieval quality degrades — every AI reads this file every session.
5. **No top-level map.** A new AI session (or Jay in six months) must infer the folder layout. `CLAUDE.md`/`SYSTEM_PROMPT.md` describe behavior, not geography.
6. **Multi-AI entry points are Claude-only.** ChatGPT/Codex and Gemini have no `AGENTS.md`/`GEMINI.md` pointing at `SYSTEM_PROMPT.md` — they rely on Jay remembering to say "read SYSTEM_PROMPT.md".
7. **No archive/lifecycle policy.** `daily/`, `notes/`, tickler Done rows, and `inbound/` grow forever. Small now (1.2 MB), but the retrieval-noise problem compounds.
8. **Small holes:** `SOL` project still "(add description)"; tickler numbering is manual and drifting; `.DS_Store` not gitignored; the "working style" knowledge about Jay (misses Jira comment notifications, writes his own comments, FYI-means-save) is scattered across three files.

---

## Phase 1 — Foundation & housekeeping (mechanical, do first)

1. **`git init`, local-only.** Extend `.gitignore` with `.DS_Store` and `inbound/` (sensitive employee data — keep out of history so a future remote can never leak it; `reference/` stays tracked but flag any new sensitive file). Initial commit. **Do NOT add a remote** — this repo contains personnel-adjacent data; local-only unless Jay explicitly sets up a private remote later.
2. **Create top-level `README.md`** — a one-screen map: what each folder is for, where a given kind of information goes (mirror the routing table in `CLAUDE.md`), and the "start here" reading order for a fresh AI session.
3. **Add `AGENTS.md` and `GEMINI.md`** — thin pointers: "Read `SYSTEM_PROMPT.md` and follow it. Project conventions in `README.md`." Keeps all three AIs on the same rails without duplicating content in three places.
4. **Restructure `knowledge/current_projects.md`:**
   - Reorder sections, renumber, keep each project to a ~10-line summary: description, status, key people, next step, Jira links, and a link to a `notes/` file for depth.
   - Move deep detail out to notes: section 7's Confluence Cloud API/token detail → `notes/confluence-cloud-api-access.md`; section 9's Finance Dashboard stack detail → `notes/finance-dashboard-opex.md`. Link back.
   - Add a status line per project: `Active / Waiting-on / Background` so briefings can prioritize.
5. **Triage `inbound/`:** add `inbound/README.md` defining the lifecycle (drop → process → move to `reference/` or delete; index processed files in `knowledge/reference-documents.md`). Then process the current backlog with Jay in-session: for each file, one line — keep/archive/delete + what action it fed. Delete the obvious dupes after confirming with Jay.
6. **Small fixes:** fill in `SOL` description (ask Jay: appears to be GDPR/user-deletion + general service ops); consolidate scattered Jay-preferences into `knowledge/working-style.md` (see Phase 2.4); fix tickler item numbering.

## Phase 2 — Capture pipelines (the "throw things in" experience)

1. **Screenshot pipeline (Jay's top ask).**
   - New folder: `inbound/screenshots/` (drop zone — filename doesn't matter).
   - New skill: `skills/process_screenshot.md`. On "process my screenshots" (or any image dropped in chat): read the image; extract source (Slack channel/DM, email, app), date, people involved, decisions, action items, and any links/ticket keys; write/update the right file (`notes/`, `knowledge/key_people.md`, `tickler.md`, daily note) using existing conventions; then move the image to `reference/screenshots/YYYY-MM-DD-topic.png` and index it in `reference-documents.md` if worth keeping, or delete it if fully absorbed (ask Jay which default he wants — recommend: keep for 90 days, then archive sweep).
   - **Advice for Jay:** screenshots work, but for Slack, *copy-pasted text beats screenshots* when it's low-effort — searchable, smaller, and the AI extracts names/links losslessly. Suggested habit: paste text when it's one message, screenshot when it's a thread/visual. Both routes land in the same pipeline.
2. **Generalize to a capture skill:** `skills/process_inbound.md` covering any file dropped in `inbound/` (CSV/XLSX/PDF/MD): summarize, extract actions, index, move to `reference/` or mark disposable. The brain-dump skill handles text; this handles files.
3. **Email + calendar, manual tier (works today, no approvals):** keep `reference/emails/` PDF convention; also accept raw pasted email text into the brain-dump flow. For calendar, Jay can paste his week ("here's my week") into the daily briefing — the assistant folds meetings into Action Items and tickler triggers.
4. **`knowledge/working-style.md`** — consolidate how Jay works and how the assistant should behave: misses Jira comment notifications (proactively surface new comments on watched tickets); writes his own Jira comments; FYI = save it; keeps big picture, needs detail on demand → answers should lead with the specific fact + where it came from; prefers being surfaced aging/at-risk items before being asked. Link from `SYSTEM_PROMPT.md` core instructions so every session reads it.
5. **Slack digests:** keep the existing Slack-AI weekly digest convention; add a recurring tickler row ("Friday: paste #ai-chat / #atlassian-cloud-migration digests") so it actually happens.

## Phase 3 — Automation & maintenance rhythms

1. ⚠️ **Scheduled morning briefing** (needs Jay's OK + a time): a scheduled task, weekdays before standup prep (~8:00 PT), that generates `daily/YYYY-MM-DD.md` — tickler matches, Ticket Watch via Atlassian MCP, carry-over from the previous note. The 7/13 briefing proves the format; this just makes it automatic.
2. **Weekly review skill** (`skills/weekly_review.md`, run Fridays or on demand): sweep tickler for stale/aging items and renumber; move Done rows older than 30 days to an archive section; list unprocessed `inbound/` files; flag `current_projects.md` sections with no update in 30+ days ("still active?"); list carry-over items that have rolled 3+ times (these are the silently-stuck ones — exactly what falls out of a disorganized-by-nature head).
3. **Archive policy:** `archive/daily/YYYY-MM/` for daily notes older than ~60 days; notes stay put (they're reference) but get a `Status: superseded` header when obsolete. Weekly review handles the sweep.
4. **Handoff discipline:** end-of-session handoff note whenever meaningful state was built (existing convention — just enforce it via `working-style.md`).

## Phase 4 — Integrations (need approvals/decisions — PREP ONLY, do not action)

1. **Slack MCP** — the real fix for "Slack has a wealth of information." Needs Dean/security approval like the Okta MCP ask. **Action:** add to tickler item #3's "ask Dean" running list (it's the same conversation). Until then: screenshots + pasted text + Slack-AI digests.
2. **Gmail/Calendar** — the Google service account **won't work** for this (service accounts can't read a user's Gmail/Calendar without domain-wide delegation, which is an org-level security grant Dean would have to approve — worth asking, but don't assume). Nearer-term options to evaluate with Jay: Anthropic's native Gmail/Calendar connectors on claude.ai (per-user OAuth, may need workspace admin allowlisting — Jay may be able to self-serve since he admins the Claude org), or a Google Workspace MCP with Jay's own OAuth creds. Add to the Dean list only if per-user OAuth is blocked by policy.
3. **Google Drive restricted-drive strategy** — Jay's instinct (copy important docs to his own Drive, share with the service account) is workable and already the de-facto pattern (On24 rollout doc). Two cautions to encode in `reference-documents.md` conventions:
   - **Copies go stale.** Every copied doc gets an index row: source-of-truth link, copy link, copied-on date. Weekly review flags copies older than ~30 days when the doc matters.
   - **Copy deliberately, per-doc, not in bulk** — the restricted drive is restricted on purpose; per-item copies of docs Jay already has legitimate access to keep the footprint small and defensible. If a whole folder is needed recurringly, the cleaner ask is getting the service account added to that specific folder — another Dean-list candidate.

---

## Suggested implementation order for the Sonnet session

1. Phase 1 items 1–3 (git, README, AGENTS/GEMINI pointers) — 15 minutes, zero risk.
2. Phase 1 item 4 (current_projects restructure) — do carefully, preserve every fact, verify links after moving detail to notes.
3. Phase 2 items 1, 2, 4 (screenshot skill, inbound skill, working-style file) + wire them into `SYSTEM_PROMPT.md` and `CLAUDE.md`.
4. Phase 1 item 5 (inbound triage) — needs Jay in the loop for keep/delete calls.
5. Phase 3 with Jay's sign-off on the schedule; Phase 4 prep = tickler updates only.

## Blockers / open questions for Jay

- Confirm git repo stays **local-only** (recommended) and `inbound/` is gitignored.
- Screenshot retention default: archive after absorption, or delete? (Recommend: keep 90 days.)
- Morning briefing: want it scheduled? What time?
- `SOL` project key description — what does it actually cover?
- OK to add Slack MCP + Drive-folder service-account access to the "ask Dean" list?

## References

- Reviewed: `SYSTEM_PROMPT.md`, `CLAUDE.md`, `tickler.md`, all of `knowledge/`, `skills/`, `templates/`, `handoff/README.md`, `daily/2026-07-13.md`, folder inventory, `.claude/settings.local.json`, `.gitignore`.
- Prior art this plan builds on: daily briefing format (`daily/2026-07-13.md`), reference-documents capture conventions, slack-digests README.
