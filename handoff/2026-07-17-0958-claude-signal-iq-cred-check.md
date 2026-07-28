# Handoff: Signal IQ credential checker

**From:** Claude (personal-assistant-agy session)
**For:** Claude CLI session run from `~/src/signal-iq/cred-check/`
**Date:** 2026-07-17

## Context

Jay is IT/helpdesk support for Theo Moser's "Signal IQ" internal dashboard
(GCP project `project-management-8310`, Cloud Run service `signal-iq`,
Jira ticket [ITOPS-44593](https://intranet.oreilly.com/jira/browse/ITOPS-44593),
API keys tracked in [AM-2301](https://intranet.oreilly.com/jira/browse/AM-2301)).
Repo: `github.com/oreillymedia/PMO-command-center` (never renamed from its
old name).

Theo tested the shared credentials Jay/Alain wired into Cloud Run and hit
two failures:

- **Jira**: shared PAT returns `401 Unauthorized` directly from Jira.
- **Productboard**: `Could not parse the provided public key` — the
  `productboard-private-key` GCP secret's PEM content looks malformed.

Jay also has a theory worth testing: there's a **basic-auth bypass rule for
Jira from certain networks** that might be intercepting the request before
it reaches Jira's own auth check, which would produce a 401 that looks like
"bad token" but isn't. Running the same auth attempt from a laptop
(presumably on-network/VPN) instead of Cloud Run isolates whether this is a
credential problem or a network/proxy problem.

## What's set up

`~/src/signal-iq/cred-check/`:
- `check_creds.py` — validates the Jira PAT (direct API call, checks for a
  `WWW-Authenticate: Basic` challenge header as the tell for the proxy
  theory) and the Productboard OAuth2 Server-to-Server JWT flow (signs the
  exact JWT it expects and exchanges it for a token, per the app's own
  setup page — `aud` is confirmed as `https://app.productboard.com/oauth2/token`,
  NOT `api.productboard.com`).
- `.env` — `op://` references for 1Password, not real values. Run via
  `op run --env-file=.env -- python3 check_creds.py [jira|productboard]`.
- `requirements.txt`, `README.md`.

**1Password items already found** (Employee vault, all created within the
last 2 days — presumably by Alain/whoever provisioned the Cloud Run env
vars):
- `app-jira-signal-iq` → field `PAT for Signal IQ AM-2301` (the Jira PAT)
- `Signal IQ API keys` → fields `Client ID`, `Public key ID`, `Customer ID`,
  `User ID`
- `Datadog API key for signal-iq` (unrelated to this task)

## Open item — needs Jay

**The Productboard private key's 1Password location is not confirmed.**
`Signal IQ API keys` has a `Public key ID` field (suggesting a keypair was
generated locally, the public half uploaded to Productboard's OAuth app,
and the private half kept separately) but no field obviously named for the
private key itself. Do not guess an `op://` path for this — the `.env` file
has a `TODO-CONFIRM-ITEM/TODO-CONFIRM-FIELD` placeholder. Either:

1. Ask Jay where the private key is stored in 1Password (check field names
   he didn't label obviously — e.g. a generically-named "password" or
   "credential" field on one of the items above), or
2. Pull it directly from the GCP secret for a one-off manual test:
   ```bash
   gcloud secrets versions access latest --secret=productboard-private-key
   ```
   and `export PRODUCTBOARD_PRIVATE_KEY="$(...)"` by hand rather than
   relying on `op run` for that one variable, until the 1Password location
   is confirmed.

## Also relevant

- `~/src/signal-iq/` already has a `workfront-private-key.zip` and a PDF
  export of the Productboard OAuth app's setup page
  (`Signal IQ AM-2301 _ Productboard.pdf`) — that PDF is where the `aud`
  value was confirmed from.
- Established project pattern for secrets: see
  `~/src/personal-assistant-agy/notes/op-cli-env-secrets-guide.md` — local
  dev secrets go through `op run`, never plaintext in `.env`. Once
  something is Cloud-Run-hosted, secrets live in GCP Secret Manager
  instead (already the case for the production app).
- Jay corrected a prior assumption: **Jira's backend/infra config (reverse
  proxy rules, network-based auth bypass) is NOT in O'Reilly's DevDocs** —
  ITOPS (Alain) owns that directly. Don't search DevDocs for Jira infra
  questions.
