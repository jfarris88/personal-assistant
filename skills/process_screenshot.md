# Skill: Process Screenshot

## Goal
Turn a screenshot (dropped in chat, or sitting in `inbound/screenshots/`) into structured, searchable knowledge — Jay's #1 capture want. This is the image-specific counterpart to `process_inbound` (files) and `process_brain_dump` (text).

## Trigger
- Jay drops an image directly into chat.
- Jay says "process my screenshots" (or similar) with files sitting in `inbound/screenshots/`.

## Instructions
1. **Read the image.** Identify: source (Slack channel/DM, email, app/dashboard, document), date (from timestamps in the image if visible, else ask or use today), people involved, decisions made, action items, and any links or Jira/Confluence ticket keys visible.
2. **Route the extracted content** using the same judgment as `CLAUDE.md`'s routing table:
   - New/updated person or team detail → `knowledge/key_people.md`
   - Project status or decision → `knowledge/current_projects.md`
   - Reminder or follow-up → `tickler.md`
   - Anything that needs a deeper writeup → a `notes/kebab-case-title.md` file
   - Always fold a one-line mention into today's `daily/YYYY-MM-DD.md` if the session has one open
3. **Present the extraction to Jay** before filing anything, the same way a brain dump gets presented — brief summary of what was read and where it's proposed to go. Once approved, save/update the files.
4. **File the image itself:**
   - If worth keeping as a reference artifact: move/save to `reference/screenshots/YYYY-MM-DD-topic.png` and add a row to `knowledge/reference-documents.md`.
   - If fully absorbed into text (nothing visual worth keeping): delete it — don't keep a duplicate of information that now lives in a file.
   - **Retention default: keep 90 days**, then the weekly review sweep (`skills/weekly_review.md`, once built) archives or deletes it. Ask Jay if a specific screenshot should be kept longer (e.g. it's a visual reference, not just data).

## Habit note (tell Jay, don't just apply silently)
For Slack in particular: **copy-pasted text beats a screenshot** when it's a single message — it's searchable, smaller, and extraction is lossless (no OCR risk on names/links). Suggest: paste text for a single message, screenshot for a thread or something visual. Both land in the same pipeline either way.

## Notes
- `inbound/screenshots/` is a pure drop zone — filename doesn't matter, don't rename before processing.
- `inbound/` (including `inbound/screenshots/`) is gitignored — it can contain sensitive content before triage. Only the final destination (`reference/screenshots/`) is version-controlled.
