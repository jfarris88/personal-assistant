# Atlassian MCP Server Setup for Claude Code

Comprehensive guide to setting up the Atlassian MCP server (Jira + Confluence) for Claude Code on a new laptop. Two approaches are documented: **PyPI (recommended, simpler)** and **Docker Compose (local control)**.

## Quick Start (PyPI Approach - Recommended)

### Prerequisites
- Claude Code installed and working
- `uvx` available in PATH (comes with Python 3.12+)
- 1Password with Atlassian credentials saved

### Step 1: Store Your PATs in 1Password

Generate Personal Access Tokens (PATs) from your Atlassian account:
1. Go to [https://id.atlassian.com/manage/api-tokens](https://id.atlassian.com/manage/api-tokens)
2. Create two tokens: one for Jira, one for Confluence
3. Save both to 1Password with readable titles

**JAY'S CURRENT TOKENS:**
- Title: `Atlassian Jira PAT (read)` 
- Title: `Atlassian Confluence PAT (read)`

(Verify which titles are used in 1Password — they may have been updated)

### Step 2: Add MCP Configuration to Claude Code

Edit `~/.claude/settings.json` or the project-specific `.claude/settings.json`:

```json
{
  "mcpServers": {
    "jira": {
      "type": "stdio",
      "command": "uvx",
      "args": [
        "mcp-atlassian"
      ],
      "env": {
        "JIRA_URL": "https://intranet.oreilly.com/jira/",
        "JIRA_PERSONAL_TOKEN": "${JIRA_PERSONAL_TOKEN}",
        "JIRA_PROJECTS_FILTER": "AI, AINATIVE, FLEX1, FLEX2, FLEX4, LABS, PLAT, PE, DAP, UPC, METACON, PUBENG, ATLAS, SE, SRE, INKA, UA, GUAC, TACT",
        "CONFLUENCE_URL": "https://intranet.oreilly.com/confluence",
        "CONFLUENCE_PERSONAL_TOKEN": "${CONFLUENCE_PERSONAL_TOKEN}"
      }
    }
  }
}
```

### Step 3: Set Environment Variables

Add to your shell profile (`~/.zshrc` or equivalent):

```bash
export JIRA_PERSONAL_TOKEN="<your-jira-pat>"
export CONFLUENCE_PERSONAL_TOKEN="<your-confluence-pat>"
```

Or use 1Password CLI to inject at runtime (preferred):

```bash
# Test the integration
eval $(op signin)
export JIRA_PERSONAL_TOKEN=$(op read 'op://Private/Atlassian Jira PAT (read)/credential' 2>/dev/null)
export CONFLUENCE_PERSONAL_TOKEN=$(op read 'op://Private/Atlassian Confluence PAT (read)/credential' 2>/dev/null)
```

### Step 4: Test the Connection

In Claude Code terminal:

```bash
uvx mcp-atlassian list_projects
```

If successful, you'll see a JSON list of Jira projects.

---

## Full Setup (Docker Approach - Local Development)

For more control, or if you want to run your own container:

### Prerequisites
- Docker Desktop running
- `docker compose` v2 in PATH
- `/Users/jfarris/src/mcp-atlassian` cloned (see below)

### Step 1: Clone the mcp-atlassian Repository

From your personal-assistant-agy project, the `mcp-atlassian` repo is referenced at:
- Main repo: `~/src/mcp-atlassian`
- Backup: `~/src copy/mcp-atlassian` (if laptop migration)

Clone from GitHub:

```bash
cd ~/src
git clone https://github.com/sooperset/mcp-atlassian.git
cd mcp-atlassian
```

### Step 2: Create `.env` File

Create `~/.../src/mcp-atlassian/.env`:

```
JIRA_URL=https://intranet.oreilly.com/jira
JIRA_PERSONAL_TOKEN=<your-jira-pat>
JIRA_SSL_VERIFY=false
CONFLUENCE_URL=https://intranet.oreilly.com/confluence
CONFLUENCE_PERSONAL_TOKEN=<your-confluence-pat>
CONFLUENCE_SSL_VERIFY=false
```

### Step 3: Configure Claude Code

Add to `~/.claude/settings.json`:

```json
{
  "mcpServers": {
    "mcp-atlassian": {
      "type": "stdio",
      "command": "docker",
      "args": [
        "compose",
        "--project-directory", "/Users/jfarris/src/mcp-atlassian",
        "run", "--rm", "-i",
        "mcp-atlassian"
      ]
    }
  }
}
```

### Step 4: Test the Docker Connection

```bash
cd ~/src/mcp-atlassian
docker compose pull
docker compose run --rm -i mcp-atlassian list_projects
```

---

## Troubleshooting

| Symptom | Solution |
|---------|----------|
| `command not found: uvx` | Install Python 3.12+ or upgrade with `pip install --upgrade uv` |
| `JIRA_PERSONAL_TOKEN not set` | Check 1Password read access; verify env var is exported |
| `Error: Cannot connect to the Docker daemon` | Open Docker Desktop |
| `env file not found` | Create `.env` in `~/src/mcp-atlassian/` with valid tokens |
| MCP server not appearing in Claude Code | Restart Claude Code after updating `settings.json` |
| `401 Unauthorized from Jira` | Regenerate PAT at https://id.atlassian.com/manage/api-tokens (tokens expire) |

---

## Reference

- **Jira MCP Server:** https://github.com/sooperset/mcp-atlassian
- **O'Reilly Jira:** https://intranet.oreilly.com/jira/
- **O'Reilly Confluence:** https://intranet.oreilly.com/confluence
- **Atlassian API Tokens:** https://id.atlassian.com/manage/api-tokens
- **Claude Code MCP Docs:** https://claude.ai/docs/mcp-setup

---

## Migration Checklist for New Laptop

- [ ] Install Claude Code
- [ ] Generate new Atlassian PATs (old ones may have expired)
- [ ] Store PATs in 1Password
- [ ] Update `~/.claude/settings.json` with MCP configuration
- [ ] Set environment variables (or use 1Password CLI injection)
- [ ] Test with: `uvx mcp-atlassian list_projects`
- [ ] Verify MCP server appears in Claude Code
- [ ] Optional: Clone `mcp-atlassian` repo for Docker approach
