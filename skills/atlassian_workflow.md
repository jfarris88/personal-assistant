# Skill: Atlassian Workflow

## Goal
To determine the appropriate Atlassian system (Jira or Confluence) for captured information and format it correctly.

## Guidelines

**When to suggest Jira:**
* The item is an actionable task.
* It requires tracking a status (e.g., To Do, In Progress, Done).
* It has an assignee or a due date.
* It represents a bug, feature request, or specific piece of work.

**When to suggest Confluence:**
* The item is reference material or documentation.
* It is a meeting summary, architectural decision, or long-form strategy.
* It is something that will be read and referenced over time, rather than "completed".

## O'Reilly Jira Project Keys

### Jay's Primary Projects (day-to-day work)
* `HD` — Help Desk: general employee support requests
* `AM` — Access Management: provisioning and deprovisioning
* `SOL` — (add description when known)
* `ITOPS` — IT Operations and Infrastructure Engineering (secondary involvement)

### Engineering Projects (GCP IAM support only)
Jay's team supports GCP IAM for these engineering projects. Tickets here are initiated by engineers; Jay references them for IAM/access work:
* `FLEX1`, `FLEX2`, `FLEX4` — Flex platform teams
* `INKA` — Inka
* `TACT` — Android app
* `GUAC` — iOS app
* `UA`, `METACON`, `UPC` — other engineering teams
* `AINATIVE`, `AI` — AI/AI Native teams
* `DAP` — Data Platform
* `PE` — Platform Engineering
* `SRE` — Site Reliability Engineering
* `SE` — Software Engineering

When routing a brain dump item: default to `HD`, `AM`, or `SOL`. Only use an engineering project key if Jay is explicitly doing GCP IAM work for that team.

## Assignment Rules
* **HD tickets** — leave unassigned. Other team members will pick them up.
* **AM, SOL, ITOPS tickets** — assign to `jfarris` unless told otherwise.

## Helpdesk Team Ownership (HD)
The Helpdesk team owns all of the following — tickets for these areas go to **HD**, not ITOPS:
* Google Workspace — all settings, configuration, access, and administration
* MDM (Iru) — Windows and Mac device management, app deployment, custom scripts
* *(add more as they come up)*

## Confluence Personal Space
* Jay's personal Confluence space key: `~jfarris`
* Home page: https://intranet.oreilly.com/confluence/pages/viewpage.action?pageId=55839932
* Use this space for personal notes, drafts, and private documentation. Pages created here are visible only to Jay by default.

## Execution
Once the user approves the suggested items:
1. Use the Atlassian MCP to create the agreed-upon Jira tickets using the `jira_ticket` template format.
2. Use the Atlassian MCP to create or update Confluence pages using the `confluence_page` template format.
3. When creating Confluence pages for personal/private use, always set the space to `~jfarris`.

## Jira Ticket Updates — IMPORTANT
**Never update, comment on, transition, or modify any Jira ticket unless Jay explicitly says to do so in that session.** Research, summarize, and draft freely — but do not touch tickets without a direct instruction like "go ahead and update the ticket" or "add that comment."

## Jira Comment Style
- Jay writes his own Jira comments — do not auto-post comments without explicit approval
- Jay will provide writing style guidance over time; update this section as preferences are shared
- When research is done that would inform a comment, present the findings and let Jay decide what to write
- Jay tends to miss Jira comment notifications (email/activity feed) — when watching a ticket, proactively check for new comments and flag them

## Link Handling
**Always use the mcp-atlassian server tools to look up and reference Jira/Confluence content.**

- **Jira ticket URL format**: `https://intranet.oreilly.com/jira/browse/{KEY}` — e.g. `https://intranet.oreilly.com/jira/browse/TOOLS-210`
- **Confluence base URL**: `https://intranet.oreilly.com/confluence/`

Use these base URLs when constructing links. When in doubt, fetch the item via the MCP tool and use the URL returned in the result.
