# Atlassian MCP / Confluence Usage Guide Access

**Timestamp:** 2026-06-17 11:20:48 PDT
**LLM / System:** OpenAI Codex (GPT-5)
**Session Topic:** Determine whether O'Reilly's ChatGPT rollout documentation answers whether WIP manuscripts are non-restricted content
**Status:** Blocked on `mcp-atlassian` connectivity in this session

---

## Summary

This session started from an archived PDF of Dean Roman's June 16, 2026 rollout email announcing ChatGPT access for all employees through Okta.

The immediate question was:

`"Hi, question regarding O'Reilly's ChatGPT...are WIP manuscripts considered non-restricted content?"`

The rollout email itself does **not** answer that directly. It says:

- ChatGPT is available through Okta
- employees should review the usage guidelines
- users should not input sensitive, confidential, or proprietary information unless explicitly permitted

The email links to a Confluence usage guide that is the most likely source of a definitive answer. This session was unable to retrieve that page because the Atlassian MCP connection available in other sessions was not functioning here.

## What Was Checked

### Email PDF and extracted links

- Confirmed the archived file: [2026-06-16-chatgpt-access-all-employees-okta-email.pdf](../reference/emails/2026-06-16-chatgpt-access-all-employees-okta-email.pdf)
- Extracted visible links from the PDF:
  - `https://oreilly.okta.com/`
  - `https://intranet.oreilly.com/confluence/x/zoHBCw`
  - `mailto:solutions@oreilly.com`
  - `https://oreilly.slack.com/archives/C0NEGH1LL`

### Confluence access attempts

- Tried the Confluence tiny URL in the in-app browser.
- It redirected to the login page rather than exposing the content in this session.

### Atlassian MCP checks

- Found local evidence that the Atlassian MCP has been used in other sessions:
  - [.claude/settings.local.json](../.claude/settings.local.json)
  - [/Users/jfarris/src/.mcp.json](/Users/jfarris/src/.mcp.json)
  - [/Users/jfarris/src/mcp-atlassian/MCP_CONNECTION.md](/Users/jfarris/src/mcp-atlassian/MCP_CONNECTION.md)
- Verified the configured MCP server name is `mcp-atlassian`.
- Ran `claude mcp list` locally and it reported:
  - `mcp-atlassian ... Failed to connect`

### Docker and server health clues

- The MCP server is configured to run via Docker Compose from `/Users/jfarris/src/mcp-atlassian`.
- A local Docker check returned a permission error on the Docker socket in this session, which is one likely reason the MCP server could not be started here.
- The environment file at [/Users/jfarris/src/mcp-atlassian/.env](/Users/jfarris/src/mcp-atlassian/.env) also appears to contain a malformed value:
  - `CONFLUENCE_SSL_VERIFY=false2`

## Current Blockers

- This Codex session does not currently have working `mcp__mcp-atlassian__...` tools mounted.
- The configured `mcp-atlassian` server fails health checks from this environment.
- Docker access is blocked in this session unless elevated access is approved.
- Even if Docker access is restored, the Confluence env configuration may still need correction before the MCP server can query Confluence successfully.

## Recommended Next Steps

- [ ] Re-check `mcp-atlassian` from a session where the MCP tools are already mounted and healthy.
- [ ] If working from this project again, repair the Atlassian MCP path first.
- [ ] Confirm Docker access is available to the session running the check.
- [ ] Fix `CONFLUENCE_SSL_VERIFY=false2` in `/Users/jfarris/src/mcp-atlassian/.env` if that value is unintended.
- [ ] Re-run `claude mcp list` and `claude mcp get mcp-atlassian` after Docker and env issues are resolved.
- [ ] Once Confluence access works, open the page behind `https://intranet.oreilly.com/confluence/x/zoHBCw` and look specifically for policy language about manuscripts, drafts, proprietary content, or publishing material.

## References

- Daily note: [2026-06-17 Daily Notes](../daily/2026-06-17.md)
- Email summary note: [ChatGPT Okta Rollout Email](../notes/chatgpt-okta-rollout-email.md)
- Archived PDF: [2026-06-16-chatgpt-access-all-employees-okta-email.pdf](../reference/emails/2026-06-16-chatgpt-access-all-employees-okta-email.pdf)
- Atlassian MCP connection notes: [/Users/jfarris/src/mcp-atlassian/MCP_CONNECTION.md](/Users/jfarris/src/mcp-atlassian/MCP_CONNECTION.md)
- Project MCP config: [/Users/jfarris/src/.mcp.json](/Users/jfarris/src/.mcp.json)
