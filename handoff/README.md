# Handoff Notes

Central place to save session handoffs so different AI systems and future sessions can quickly pick up work without re-discovering context.

## Naming Convention

Use:

`YYYY-MM-DD-HHMM-llm-topic.md`

Examples:

- `2026-06-17-1120-openai-codex-gpt5-mcp-atlassian-confluence.md`
- `2026-06-17-1545-claude-code-google-workspace-migration.md`

Rules:

- Use local time in `HHMM` 24-hour format.
- Include the LLM or tool family in the filename.
- Keep the topic short, specific, and kebab-case.

## Required Contents

Each handoff note should include:

- timestamp
- LLM / system used
- session topic
- current status
- summary of work completed
- blockers or unresolved issues
- recommended next steps
- references to local files, links, tickets, or docs

## Suggested Workflow

- Create a new handoff note when pausing work that another AI or future session may resume.
- Link the handoff from the relevant daily note if the session materially affected current work.
- Prefer factual summaries over long transcripts.
