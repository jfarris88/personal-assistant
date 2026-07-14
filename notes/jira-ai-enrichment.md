# Jira AI Ticket Enrichment — Idea Spec

## Concept

After a Jira ticket is marked Done, trigger an AI pass that writes a structured Resolution Summary back to the ticket. This makes completed tickets more searchable and reusable when similar issues arise later. It also feeds the broader **IT triage workflow** — when a new problem arrives, the enriched ticket history plus reference sources like DevDocs form the research corpus the AI works from.

Starting fresh (new tickets going forward) rather than backfilling — enrichment quality depends on comments and context being present, which old closed tickets often lack.

---

## What the Enrichment Produces

A standardized **Resolution Summary** comment posted to the ticket containing:

- **Root cause** — what was actually wrong
- **Fix applied** — what was done and why
- **Related tickets** — similar past issues (linked)
- **Tags/Labels** — normalized terms for search (e.g., `okta`, `mdm`, `offboarding`, `access-review`)
- **Recurrence risk** — low/medium/high, with brief rationale

---

## Architecture

### 1. Trigger
Jira automation rule: when ticket status transitions to **Done**, fire a webhook to an enrichment service.

Scope to start: **ITOPS, HD, SYSPER** (highest recurrence value)

### 2. Enricher (Claude-powered)
Reads from the ticket:
- Summary + description
- All comments (chronological)
- Linked issues and PRs
- Work log (if present)

Additionally consults **DevDocs** (`~/src/devdocs`) for platform context:
- Relevant runbooks (e.g., Datadog on-call runbook for infra issues)
- Architecture docs for the affected system (auth, accounts, search, etc.)
- Known security or vulnerability patterns

The enricher uses DevDocs to add a **Platform Context** section when relevant —
e.g., linking to the runbook that covers this failure mode, or noting which
platform component owns the affected surface.

Sends to Claude with a prompt that instructs it to produce the structured Resolution Summary. No hallucination of facts — it summarizes only what's in the ticket and DevDocs.

**Draft prompt (system):**
```
You are an IT operations knowledge base assistant at O'Reilly Media. You will
be given the full content of a completed Jira ticket (description, comments,
linked issues) and optionally relevant excerpts from internal Engineering
documentation (DevDocs). Produce a Resolution Summary with these sections:
Root Cause, Fix Applied, Platform Context (if DevDocs excerpts are relevant),
Related Tickets, Suggested Labels, and Recurrence Risk (low/medium/high with
one sentence of reasoning). Base your summary only on information present in
the ticket and provided documentation. Use plain language, no jargon inflation.
```

### 3. Write-back
Post the Resolution Summary as a Jira comment with a header like:
> 🤖 **AI Resolution Summary** *(auto-generated — edit or delete as needed)*

Optionally: apply suggested labels to the ticket automatically (requires permission check).

---

## Implementation Options

| Approach | Pros | Cons |
|---|---|---|
| Webhook → Lambda/Cloud Function | Clean, event-driven, no polling | Needs hosting + secret management |
| Jira automation → n8n/Make | No-code friendly, easy to iterate | Another tool to maintain |
| Scheduled batch (daily) | Simple, can batch cheaply | Enrichment is stale by hours |
| Claude Code scheduled task | Already in this stack | Requires Jira API polling |

**Recommended starting point:** Jira automation webhook → a small Python script (or n8n flow) → Anthropic API → Jira comment API.

---

## Jira API Calls Needed

```
GET  /rest/api/3/issue/{issueKey}          # full ticket content
GET  /rest/api/3/issue/{issueKey}/comment  # comments
POST /rest/api/3/issue/{issueKey}/comment  # write-back
PUT  /rest/api/3/issue/{issueKey}          # apply labels (optional)
```

Auth: existing `jfarris` Jira API token or a dedicated service account.

---

## Triage Research Sources

When investigating a new problem, the AI should pull context from:

| Source | What it covers | How to access |
|---|---|---|
| Enriched Jira history | Past resolutions for similar tickets | Jira search API |
| **DevDocs** | Platform architecture, runbooks, auth, security | `~/src/devdocs/devdocs/docs/` — local clone |
| Datadog | Live monitoring, on-call runbook | Datadog Notebook #1789523 |
| Confluence | Team-specific runbooks and KB articles | Confluence space search |
| `#platform-issues` Slack | In-flight incident context | Slack API (future) |

DevDocs key sections for IT triage:
- `docs/operations/monitoring/runbooks.md` — what to do when something's on fire
- `docs/operations/security/` — OWASP, vulnerabilities, pentesting, SOC
- `docs/accounts-and-users/` — user/account domain model
- `docs/authentication/` — JWT, auth patterns
- `docs/infrastructure/` — platform infra overview

---

## Open Questions

- [ ] Use a dedicated Jira service account or personal token?
- [ ] Auto-apply labels or only suggest them in the comment?
- [ ] Write back to Confluence KB as well, or just the ticket?
- [ ] Which projects to scope to first — ITOPS+HD, or broader?
- [ ] Who reviews/edits the summaries before they're useful? (Or is self-correction on next occurrence fine?)
- [ ] How to chunk/index DevDocs for efficient retrieval at enrichment time — full-text search vs. vector index vs. keyword routing by ticket label?

---

---

## Inbound Enrichment — Reporter Context (Pod Exec / Access Requests)

### Concept

When a new ticket arrives in the **SOL** project (and potentially HD/AM), automatically look up the reporter's department, engineering team, and manager — then post that as a **restricted comment visible only to IT/Administrators**, not the requester. Gives the help desk team instant context without asking the reporter or searching manually.

Sparked by SOL-106841 (Jamey Nakama, pod exec request, 2026-06-30) — the team had to manually verify team context and legitimate use case via the linked FLEX ticket.

---

### What the Restricted Comment Contains

```
🔒 Reporter Context (IT Team Only)
Department:       Engineering — Platform Team
Manager:          [Manager Name]
Team roster ref:  [Engineering Team Google Sheet link]
```

---

### Data Sources

- **Engineering Team Google Sheet** — org-level roster mapping employee → team + manager
- **IT Roster** (internal) — secondary source if Google Sheet doesn't cover all employees

The enricher should do a name/email lookup against these sources and fall back gracefully if the reporter isn't found (non-engineering requesters, contractors, etc.).

---

### Jira Restricted Comment

Jira Server/DC supports visibility restrictions on comments via the `visibility` field in the comment API:

```json
POST /rest/api/2/issue/{issueKey}/comment
{
  "body": "🔒 Reporter Context (IT Team Only)\n...",
  "visibility": {
    "type": "role",
    "value": "Administrators"
  }
}
```

This makes the comment invisible to the reporter and visible only to users with the specified Jira role. Confirm the exact role name to use (`Administrators`, `Service Desk Team`, etc.) in the O'Reilly Jira instance.

---

### Trigger

- **When:** Ticket created in SOL (and optionally HD, AM)
- **Filter:** All tickets, or scoped to specific issue types (e.g., Support Request)
- **Method:** Jira automation webhook → enrichment script → restricted comment write-back

---

### Open Questions

- [ ] What is the exact Jira role name to use for the visibility restriction?
- [ ] Is the Engineering Team Google Sheet accessible via API (Sheets API service account)?
- [ ] Does the roster cover non-engineering staff (G&A, Sales) or just eng?
- [ ] Should this run on all SOL tickets or only specific templates (e.g., pod exec form-generated tickets)?
- [ ] Where does the roster live and how current is it — is it maintained or stale?

---

## Status

**Idea / Not started** — filed 2026-06-24

**Inbound Enrichment sub-idea** added 2026-06-30
