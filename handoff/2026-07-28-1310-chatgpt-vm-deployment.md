# Handoff: ai-usage-tracker moved to orm-helpdesk VM (for ChatGPT, which helps maintain export_chatgpt_enterprise_usage.py)

**Date:** 2026-07-28
**For:** ChatGPT, which Jay uses to help write/maintain `export_chatgpt_enterprise_usage.py`. This
covers infra changes made around that script this week — where it now runs, how it gets its
credentials, and how to deploy a change.

---

## What changed

The whole `ai-usage-tracker` project (previously only run from Jay's laptop via cron) now also
runs on a GCP VM, `orm-helpdesk` (project `helpdesk-prod-1`), and lives in a new git repo:
**https://github.com/oreillymedia-corporate/ai-usage-tracker** (private).

Both `export_claude_enterprise_usage.py` and `export_chatgpt_enterprise_usage.py` run hourly on
that VM via cron (`20 * * * *`, i.e. every hour at :20 past), triggered through a wrapper script,
**`run_on_vm.sh`**, which is VM-only and checked into the repo alongside the exporters.

## How credentials work now (VM only — laptop dev is unchanged)

On the laptop, `export_chatgpt_enterprise_usage.py` still works exactly as before: `.env` for
`CHATGPT_WORKSPACE_ID`, and the Admin key either from `OPENAI_CHATGPT_ADMIN_KEY` or (default)
read live from 1Password (`ChatGPT Enterprise API key for Analytics (AM-2480)`, `Helpdesk` vault)
via the `op` CLI. **None of that changed.**

The VM has no 1Password CLI installed, so instead `run_on_vm.sh` pulls everything from **Google
Secret Manager** (project `helpdesk-prod-1`) using the VM's own ambient service-account identity
— no key files, no `.env` on the VM at all:

- `AI_USAGE_TRACKER_ANTHROPIC_ANALYTICS_KEY`
- `AI_USAGE_TRACKER_ANTHROPIC_MEMBERS_KEY`
- `AI_USAGE_TRACKER_GOOGLE_SERVICE_ACCOUNT_KEY` (the Sheets service-account JSON)
- `AI_USAGE_TRACKER_OPENAI_CHATGPT_ADMIN_KEY` — seeded once from the same 1Password item above;
  `run_on_vm.sh` fetches it and exports it as `OPENAI_CHATGPT_ADMIN_KEY` before running the script,
  so `export_chatgpt_enterprise_usage.py` itself needed **zero code changes** — it just sees the
  env var set and skips the 1Password lookup, per its existing `load_admin_key()` fallback logic.

`CHATGPT_WORKSPACE_ID` isn't secret, so it's just hardcoded in `run_on_vm.sh`
(`94494ec0-1243-4cc8-885f-e93a614ac920`), same value as the laptop `.env`.

## Repo cleanup (also relevant if ChatGPT is reading the repo for context)

The repo was cleaned up before pushing: early planning/dev docs from before Claude/ChatGPT
Enterprise existed (COST_TRACKING_FINDINGS.md, GOOGLE_SHEETS_EXPORT_PLAN.md, etc.) and superseded
scripts (`sync_anthropic_spend.py`, `show_dashboard.py`, old cron helper scripts) were deleted.
The README was rewritten to describe only what's actually running. If ChatGPT has old context
referencing any of those removed files/docs, that context is stale now.

## What this means for future ChatGPT-assisted changes to the exporter

1. Local dev/testing is unchanged — edit `export_chatgpt_enterprise_usage.py`, run it locally
   with `.env` + 1Password as always.
2. To ship a change: commit, `git push github main` (or just `git push` once a remote named
   `origin`/`github` is set up in a fresh clone), then on the VM:
   ```
   ssh orm-helpdesk
   cd ~/ai-usage-tracker
   git pull   # needs a GitHub token in the URL or a configured credential helper — ask Jay/Claude
   ```
3. **Do not** add new required env vars without also updating `run_on_vm.sh` to source them (from
   Secret Manager if secret, hardcoded if not) — otherwise the VM run will fail even though the
   laptop run works fine via `.env`.
4. The script itself should keep working identically in both environments as long as it only
   reads from `os.environ` / `.env` — that's the seam that makes the same code run in both places
   without a VM-specific code path.

## Not yet done

Jay's laptop crontab still has its own entry running `export_claude_enterprise_usage.py` (not the
ChatGPT script) every hour — that's a separate, still-open cleanup item, not blocking anything
here.
