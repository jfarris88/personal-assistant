# Claude Code Instructions — Personal Assistant Project

This project is Jay Farris's personal assistant system at O'Reilly Media. Read `SYSTEM_PROMPT.md` for the full system context and workflow.

## Key Behavior Rules

### Save Context to the Project
This project is worked on by multiple AI systems — **Claude, Gemini, and ChatGPT**. The project folder is the shared source of truth for all of them. When Jay mentions anything worth remembering — a colleague's role, a recurring meeting, a team process, a tool or system name, a project detail — **save it to the appropriate file in this project folder**, not just to Claude's internal memory. Use your judgment on where it belongs:

- **People, roles, meetings, teams** → `knowledge/key_people.md`
- **Active projects and initiatives** → `knowledge/current_projects.md`
- **Reference docs, tools, systems** → `knowledge/reference-documents.md`
- **Links to read/investigate later** → `knowledge/research-links.md`
- **Session handoffs to other AI systems** → `handoff/YYYY-MM-DD-HHMM-llm-topic.md`
- **One-off notes** → `notes/kebab-case-title.md`

The goal is that any AI system (or Jay himself) opening this project should have full context without relying on Claude's session memory.

### Tickler / Reminders
When Jay asks to be reminded about something, add it to `tickler.md` using the existing format, and also set a scheduled task if a specific time is requested.
