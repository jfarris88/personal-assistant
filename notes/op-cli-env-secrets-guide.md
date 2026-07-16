# 1Password CLI (`op`) for `.env` Files

*Step-by-step for replacing plaintext credentials in `.env` files with 1Password references.*

## The problem this solves

A typical `.env` file holds real secrets in plaintext on disk:

```
API_KEY=<some long secret string>
DB_PASSWORD=<some plaintext password>
```

Anyone with filesystem access, a backup tool, a misconfigured `.gitignore`, or a laptop that gets imaged/wiped can expose these. The fix: keep the real secret in 1Password, and have `.env` hold only a **pointer** to it (`op://vault/item/field`). The actual value is fetched at runtime and never touches disk.

This is a proven setup already in use, not a theoretical one.

## Scope: local dev now, Secret Manager once it's hosted

This guide covers **local development on a laptop** — that's where `op`/1Password fits. The plan is:

- **Local dev:** run everything through `op run` as described below, whether you're hand-writing code or having an AI assistant build it for you. Secrets live in 1Password, never in plaintext on the laptop.
- **Once a project is deployed/hosted (Google Cloud):** secrets move to **Google Secret Manager** instead. The app itself shouldn't need to change — if it's written to just read plain environment variables (the standard practice either way), then locally those env vars get populated by `op run` resolving `op://` references, and in GCP they get populated by Secret Manager (e.g. via Cloud Run's built-in secret-to-env-var mounting, or a startup step that pulls from Secret Manager). Same app code, different source for the values depending on where it's running.
- This guide only covers the local/`op` half. The Secret Manager side is a separate, standard GCP setup once something is ready to be hosted.

## Prerequisites

1. **1Password desktop app** installed and signed in to the O'Reilly account.
2. **1Password CLI**:
   ```
   brew install --cask 1password-cli
   ```
3. **Enable CLI integration** in the desktop app: *Settings → Developer → "Integrate with 1Password CLI"* (toggle on). This lets `op` unlock via the desktop app's biometric/session unlock instead of prompting for your master password every time.
4. Verify it works:
   ```
   op vault list
   ```
   You should see your vaults (e.g. `Employee`, `Helpdesk`) without being dropped into a separate CLI login flow — it piggybacks on the desktop app's unlocked session.

## Step 1 — Put the secret in 1Password (if it isn't already)

Create or reuse an item in the appropriate vault (e.g. `Employee` for personal/individual API keys, or a shared team vault for shared service credentials). Store the secret as a field on that item — e.g. a field named `credential` or `token`, or `password` if it's a login-type item.

For a secret that's a **file** (like a service account JSON key) rather than a short string, attach the file to the 1Password item instead of pasting its contents into a field.

## Step 2 — Reference it in `.env`, not the raw value

Instead of:
```
API_KEY=<some long secret string>
```

Write:
```
API_KEY=op://Employee/Some Service API Key/credential
```

The format is `op://<vault>/<item name>/<field name>`. For a file attachment, the last segment is the filename instead of a field name:
```
SERVICE_ACCOUNT_JSON=op://Employee/some-service-account/service-account.json
```

Non-secret values (spreadsheet IDs, URLs, port numbers) can stay as plain values in the same `.env` file — only the actual credentials need the `op://` treatment.

## Step 3 — Run the app through `op run` instead of loading `.env` directly

Wherever the app/script currently does something like `source .env` or a library auto-loads `.env` (e.g. `dotenv`), instead run the command wrapped in `op run`:

```
op run --env-file=.env -- node index.js
op run --env-file=.env -- python main.py
op run --env-file=.env -- npm start
```

`op` reads `.env`, resolves every `op://` reference by pulling the live value from 1Password, injects the *real* values into the environment for that one process, and runs the command. The resolved secrets exist only in that process's memory — never written back to `.env` or to disk.

## Step 4 — Confirm no real secrets remain on disk

- Eyeball the `.env` file — every credential line should start with `op://`, not a real-looking token or password.
- Because `.env` no longer contains actual secrets, it's *safer* to commit than before — but keep it gitignored anyway as a habit/defense-in-depth, since plain values can still creep back in during edits.

## If an AI assistant is writing the code (vibe coding)

If you're having Claude, ChatGPT, Cursor, or similar build or modify the project, tell it up front to follow this pattern rather than writing real secrets into files — AI assistants left to their own defaults will happily hardcode a key or drop it straight into `.env` as plain text. A prompt like this works well:

> This project's secrets should never be written in plaintext. Any API key, password, or credential goes into 1Password, and `.env` should only ever contain `op://vault/item/field` references to it — never the real value. Run the app via `op run --env-file=.env -- <command>` rather than loading `.env` directly. If you need a new secret, ask me for it rather than inventing a placeholder and leaving it in the code.

A few things worth calling out to the assistant specifically:
- **Don't let it print resolved secrets to the terminal or into log/debug output** — if it needs to confirm a value works, have it check the exit code or a masked response, not echo the secret.
- **Don't let it commit a `.env` with real values** even temporarily "to test" — if it wrote one to test locally, have it swap back to `op://` references before finishing.
- If the assistant reaches for a `.env.example`/`.env.template` file to document required variables, that's fine and encouraged — it should contain placeholder text (`your_api_key_here`), never a real key or an `op://` path to a personal vault item that others won't have access to.

## Troubleshooting

- **`op` prompts to sign in every time**: CLI integration toggle (Prerequisites, step 3) isn't on, or the desktop app got signed out.
- **"item not found"**: vault name or item name in the `op://` reference doesn't exactly match what's in 1Password — names are case-sensitive and must match exactly, including spaces.
- **Field name mismatch**: open the item in 1Password and check the exact field label; `password` vs `credential` vs a custom field name are common gotchas.
