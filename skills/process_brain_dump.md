# Skill: Process Brain Dump

## Goal
To take unstructured stream-of-consciousness input from the user and organize it into actionable items and structured documentation.

## Instructions
When the user provides a "brain dump":
1. **Analyze:** Read through the text carefully to understand the context, main ideas, and any implied tasks.
2. **Extract Tasks:** Identify anything that sounds like an action item. For each task, note any implied due date or priority. Format as `- [ ] Task description 📅 YYYY-MM-DD` when a date is present.
3. **Extract Insights:** Identify key decisions made, important information learned, or ideas generated. Summarize these.
4. **Draft Structure:** Present the organized information back to the user formatted clearly with headings for "Action Items" and "Key Insights/Notes".
5. **Suggest Next Steps:** Suggest which items should become Jira tickets, which should become Confluence pages, and which are notes-only (local file only), based on the `atlassian_workflow` skill.
6. **Save locally:** Once the user approves, save files to the appropriate local folder:
   - **`daily/YYYY-MM-DD.md`** — create or append to today's daily note using the `daily_note` template. This is always created, even if nothing goes to Atlassian.
   - **`notes/kebab-case-title.md`** — for reference material that also goes (or could go) to Confluence. Use the `note` template.
   - **`jira-drafts/PROJ-kebab-case-title.md`** — for tickets being staged or already pushed. Use the `jira_draft` template. After pushing to Jira, update the `Jira key:` field with the real ticket key and set `Status: Pushed`.
   - **`knowledge/research-links.md`** — central index for saved web links, articles, and external docs the user wants to revisit. Add each new future-read link here and cross-link it from the daily note and any supporting `notes/` file.

## Due Date Handling
- If the user mentions a specific date, use it directly.
- If they say "end of week", infer the coming Friday.
- If they say "today" or "urgent" with no date, use today's date.
- If no date is implied, leave the 📅 field off — don't invent deadlines.
- Always use ISO format: `📅 YYYY-MM-DD`
