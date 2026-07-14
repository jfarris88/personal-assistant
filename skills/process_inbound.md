# Skill: Process Inbound Files

## Goal
Give any file dropped in `inbound/` (CSV, XLSX, PDF, MD, etc.) the same "capture is easier than not capturing" treatment as a brain dump or a screenshot. This handles files; `process_brain_dump` handles text; `process_screenshot` handles images.

## Trigger
- Jay says "process inbound" (or similar), or references a specific file sitting in `inbound/`.
- A new file appears in `inbound/` during a session and Jay asks what to do with it.

## Instructions
1. **Open and read the file.** Summarize what it is: type, source/author if known, date, and what it's for.
2. **Extract anything actionable:** tasks, decisions, follow-ups, people/roles mentioned. Route using the same table as `CLAUDE.md`:
   - People/roles/teams → `knowledge/key_people.md`
   - Project detail → `knowledge/current_projects.md`
   - Reminders/follow-ups → `tickler.md`
   - Deeper reference material → a `notes/kebab-case-title.md` file
3. **Present the summary and proposed routing to Jay before filing or moving anything** — same approval flow as a brain dump.
4. **Disposition the file itself**, once approved:
   - **Keep as reference:** move to `reference/` (with a sensible stable filename — no spaces, descriptive) and add a row to `knowledge/reference-documents.md`.
   - **Fully absorbed / disposable:** delete it from `inbound/` — the information now lives in a tracked file, keeping the raw export around just adds noise and (often) unnecessary sensitive data at rest.
   - **Needs Jay's call:** if it's ambiguous (e.g. contains sensitive employee data, or it's unclear whether it'll be needed again), ask rather than guessing. Default to *not* deleting until Jay confirms.
5. **Note that it's processed.** If the file stays (moved to `reference/`), its `reference-documents.md` row is the record. If deleted, no further tracking needed — the extraction now lives in whatever file it fed.

## Notes
- `inbound/` is gitignored — nothing dropped there is version-controlled until it's moved to `reference/`. Treat everything in it as potentially sensitive (rosters, exports, MFA/compliance lists have landed here before).
- See `inbound/README.md` for the lifecycle this skill implements, and `skills/weekly_review.md` (once built) for the periodic sweep of anything left unprocessed.
