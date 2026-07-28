# Handoff: Project Agy Improvement Plan

- **Timestamp:** 2026-07-14 08:11 PT
- **LLM / system:** Claude Code (Fable 5) — plan authored for implementation by a **Sonnet session**
- **Topic:** Review of the personal-assistant project folder + phased improvement plan
- **Status:** Living plan — original Phases 1–2 were implemented on 2026-07-14; plan expanded after a Codex review on 2026-07-17. Remaining work is not yet approved for implementation.
- **Scope note for future implementing sessions:** Use the 2026-07-17 addendum and revised implementation order at the end of this file as the current plan. Items involving external systems, schedules, credential rotation, retention/deletion, backup destinations, or policy approvals still require Jay's explicit approval.

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

---

## 2026-07-17 Codex Review Addendum

### Current assessment

The repository now has a strong assistant-notebook foundation: explicit working-style guidance, shared context across Claude/Gemini/Codex, low-friction capture conventions, local Git history, and useful proactive daily briefings. The next stage is to make it a **trustworthy assistant operating system** by mechanically enforcing safety, canonical task state, freshness, retention, and connector boundaries.

Do not restart or redo the completed July 14 work. Build on it. The main remaining risk is not a missing folder or template; it is that documented rules and state can contradict or drift without detection.

### New Phase 0 — Safety and authority boundaries (do before further automation)

1. **Create explicit operating modes.** Add a small authority model to `SYSTEM_PROMPT.md` and have every workflow use it:
   - **Read-only** — inspect, answer, review, or diagnose; do not write local files or mutate external systems.
   - **Capture** — save approved context locally; do not mutate external systems.
   - **Draft** — prepare proposed Jira/Confluence/messages/changes locally for review.
   - **Execute** — perform only the specific external actions Jay explicitly approved in the current session, then record receipts/results locally.
   - A user's explicit “don't act,” “review only,” or equivalent always selects Read-only and must override automatic daily-file creation or other default writes.
2. **Remove the NHT contradiction immediately.** `SYSTEM_PROMPT.md` correctly makes the NHT Jira project completely off-limits, but `templates/jira_ticket.md` currently routes “New hire or termination” to `NHT`. Remove that routing instruction and replace it with an explicit prohibition: acknowledge only context Jay supplies, never query/create/read/update an NHT ticket, and route any permitted adjacent work without opening the NHT item itself.
3. **Centralize external-mutation rules.** Keep one authoritative rule block for Jira, Confluence, messages, scheduled tasks, file deletion, and credential changes. Skills and templates should link to that block rather than restating it with subtle differences.
4. **Use staged execution for external changes.** Save/preview the intended change first, get approval, execute idempotently where possible, and record the returned key/URL/status. This avoids partial state when an external write succeeds but the local audit update fails.

### New Phase 1 — Secrets, data classification, and recovery

1. **Move the current Confluence API token out of plaintext.** The untracked `.env` contains a literal `CONFLUENCE_API_TOKEN` and is mode `0644`. It has not been committed and the repo has no remote, but it still conflicts with `notes/op-cli-env-secrets-guide.md`.
   - Replace the token with an `op://` 1Password reference.
   - Change `.env` permissions to `0600`.
   - Add a safe `.env.example` containing variable names and placeholders only.
   - Consider rotating the existing long-lived token after migration, especially if its prior exposure is uncertain.
   - Never print the current or replacement value during migration or verification.
2. **Make ignore rules portable.** Add `.claude/settings.local.json` to this repository's `.gitignore` rather than relying on Jay's global Git ignore. Keep `.env`, raw inbound material, and local AI-client state excluded.
3. **Define data classes and allowed destinations.** Suggested classes:
   - Public
   - Internal
   - Confidential
   - Personnel-sensitive
   - Secret
   For each class, document whether it may be processed by each AI/tool, stored in Markdown, placed in `reference/`, committed to Git, backed up, or retained at all. NHT remains prohibited regardless of classification.
4. **Add a pre-commit secret/data gate.** Check for common token/private-key patterns, accidental `.env` files, raw inbound exports, prohibited NHT URLs/content, and files lacking required sensitivity metadata before they enter Git history.
5. **Add real disaster recovery.** Local Git provides version history but is not a backup when the working tree and `.git` directory live on the same disk. Choose an approved encrypted backup destination and retention policy before adding a remote. Do not copy personnel-sensitive or secret material to an unapproved cloud service.
6. **Adopt a checkpoint rhythm.** After Jay-approved local changes, create a small reviewed commit at a natural session boundary. Never auto-commit unresolved, sensitive, or unrelated user changes.

### New Phase 2 — One canonical source for actions and current state

1. **Create a canonical action registry.** Tasks currently drift between daily notes, `tickler.md`, project summaries, handoffs, and Jira. Use a Markdown table, YAML, or JSONL file that remains human-readable and gives every action a stable ID. Suggested fields:
   - ID
   - Summary
   - Status
   - Priority
   - Owner
   - Next action
   - Due date
   - Trigger/review date
   - Waiting on
   - Jira/Confluence key or URL
   - Source
   - Created date
   - Last verified
   - Sensitivity
2. **Make daily briefings generated views, not competing task stores.** The daily note should select due, triggered, stale, recently changed, and carry-over actions from the canonical registry. Completing an item once should update its canonical record instead of requiring edits in multiple daily files and `tickler.md`.
3. **Fold tickler behavior into canonical actions.** Preserve date- and topic-triggered reminders, but store the trigger on the same action record. Stop using row numbers as identity; renumbering a Markdown table should never change an item's stable ID.
4. **Separate current facts from history.** Treat these as authoritative in descending order:
   - safety/authority policy
   - canonical systems, projects, people, and actions records
   - supporting notes/reference documents
   - daily briefings and handoffs as historical snapshots
   Search/retrieval instructions should prefer current-state records and use historical files only for chronology or provenance.
5. **Add freshness/provenance fields.** Current-state claims should include `last_verified`, source/link, and confidence (`confirmed`, `inferred`, or `unverified`). Use ISO dates instead of words such as “today,” “tomorrow,” or “next week” in canonical files.
6. **Turn `current_projects.md` into a compact index.** It has begun drifting again (`X`, `13.5`, `14`, then `13`) and several entries exceed the intended summary size. Keep an index with status, priority, next action, owner, last verified, and a link to one project-detail note. Load detailed files only when the topic requires them.
7. **Mark superseded historical documents.** The original section of this plan still contains statements such as “No version control” because it records the pre-implementation review. Add `superseded_by`/completion metadata to old plans and handoffs so an AI cannot mistake an accurate historical statement for current state.

### New Phase 3 — Make capture and maintenance operational

1. **Add an inbound manifest.** The original 13-file backlog still remains. Track every inbound artifact with filename/hash, received date, source, sensitivity, processing status, derived notes/actions, disposition, and disposition date.
2. **Retain a minimal disposition record even after deletion.** The current inbound skill says no record is needed once a file is deleted. Keep enough metadata to know that it was reviewed, what it produced, and why it was deleted—without retaining its sensitive contents.
3. **Finish `skills/weekly_review.md`.** It is referenced by both inbound/screenshot skills but does not exist. It should:
   - surface unprocessed inbound items;
   - find overdue, stale, or repeatedly carried actions;
   - flag projects not verified within their review window;
   - apply approved screenshot/reference retention rules;
   - detect copied reference documents that may have gone stale;
   - produce a proposed cleanup report before deleting, moving, or archiving anything.
4. **Prove the screenshot flow end to end.** No screenshot has yet reached `reference/screenshots/`. Run a small, non-sensitive example through drop, extract, approve, route, index, and disposition; refine the skill based on the actual friction.
5. **Align the daily template with the actual contract.** `SYSTEM_PROMPT.md` says the briefing has three sections but lists four, while the template omits newer sections such as 1:1 changes and Ticket Watch. Make the template and contract agree, and make optional sections capability-aware.
6. **Do not force a full context load for every interaction.** Generate a compact active-context/index view containing guardrails, current priorities, and stable aliases. Load detailed projects, people, notes, and tool playbooks on demand. This keeps startup fast as the repository grows without sacrificing granular retrieval.

### New Phase 4 — Systems registry and cross-AI portability

1. **Create one systems/connectors registry.** `skills/atlassian_workflow.md` still has a placeholder SOL description and hard-codes the Data Center `~jfarris` Confluence space, while current work also uses a Cloud personal space. Record, in one authoritative file:
   - system/environment name (Jira DC, Confluence DC, Confluence Cloud, Google Drive, etc.);
   - canonical base URL, site/space/project IDs, and purpose;
   - available connector/tool for Claude, Gemini, and Codex;
   - read/write capabilities and approval requirements;
   - prohibited uses;
   - last successful verification and known fallback.
2. **Make workflows capability-aware.** “Use the Atlassian MCP” is not sufficient when a connector may be unavailable, expired, or named differently across clients. Detect capability, report freshness/provenance, and fall back to drafting or local research instead of silently skipping required checks.
3. **Use vendor-neutral core playbooks with thin adapters.** Keep the behavioral contract and schemas shared. Let `CLAUDE.md`, `GEMINI.md`, and `AGENTS.md` explain only platform-specific loading/tool differences so the shared rules do not fork.
4. **Clarify Confluence targets during migration.** Require an explicit environment/space choice when both Data Center and Cloud could be valid. Never assume `~jfarris` alone identifies the correct destination.

### New Phase 5 — Self-checks and behavioral evaluation

1. **Add a local `doctor`/validation command.** It should be read-only by default and check:
   - broken relative Markdown links (the review found one in `notes/confluence-cloud-migration-candidates.md`);
   - missing files referenced by playbooks, including `skills/weekly_review.md`;
   - duplicate action IDs and malformed dates;
   - stale relative-time language in canonical files;
   - inconsistent project numbering/status fields;
   - templates that contradict safety policy;
   - plaintext secrets and unsafe file permissions;
   - sensitive or ignored files staged for Git;
   - stale inbound items and uncheckpointed approved work.
2. **Add cross-AI behavioral scenarios.** Run the same small test set through Claude, Gemini, and Codex and compare expected behavior:
   - “FYI” context capture;
   - a read-only repo review;
   - an NHT reference;
   - a screenshot or inbound file;
   - conflicting current and historical facts;
   - an overdue/topic-triggered reminder;
   - a Jira draft without approval;
   - an explicitly approved Jira update;
   - an unavailable/expired connector.
3. **Define success criteria.** The assistant should consistently cite the source and last-verified date, avoid duplicate tasks, preserve safety boundaries, ask only when a real judgment is required, and provide an external-action receipt after execution.

### New Phase 6 — Proactive automation only after the trust layer

1. Schedule morning briefings only after canonical actions, freshness rules, connector detection, and read-only/execute modes are working.
2. Schedule the weekly review only after retention/deletion proposals are reviewable and non-destructive by default.
3. Add Slack, Gmail, Calendar, Drive, Okta, or other connectors only with the approvals and data-classification rules already called out in the original Phase 4.
4. Keep automation observable: every run should state what sources were checked, what was unavailable, what changed locally, and whether any external mutation occurred.

### Revised implementation order (supersedes the original ordering above)

1. **Safety hotfixes:** remove the NHT template contradiction; add operating modes and centralized mutation rules.
2. **Secret hardening:** migrate the Confluence token to 1Password references, tighten permissions, add portable ignores and secret checks. Credential rotation requires Jay's explicit approval.
3. **Canonical state:** introduce stable action records, provenance/freshness fields, source precedence, and the compact project index.
4. **Operational maintenance:** add the inbound manifest, weekly review, aligned daily template, and retention proposals.
5. **Reliability tooling:** build the local doctor command, then fix issues it reports and add cross-AI behavioral scenarios.
6. **Systems portability:** centralize connector/environment metadata and thin per-AI adapters.
7. **Recovery:** choose and configure an approved encrypted backup/checkpoint approach.
8. **Automation/integrations:** schedule briefings/reviews and pursue new connectors only after the trust layer is demonstrated.

### What not to add yet

- No vector database or embeddings layer—the repository is still small enough for well-structured Markdown plus targeted loading.
- No custom web application solely to display the same information already readable in Markdown.
- No broad Slack/Gmail/Drive ingestion before classification, retention, and authority rules are enforceable.
- No automatic Jira/Confluence mutation based only on inferred intent.

The design target remains: **capture should be effortless for Jay, while state changes, sensitive-data handling, and external actions should be explicit, traceable, and dependable.**
