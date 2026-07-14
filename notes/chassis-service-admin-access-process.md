# Chassis Service Admin Access Process (Groot/Django)

General process for granting a user access to a Chassis-based microservice's Django Admin Site (URLs like `https://<service>.platform.gcp.oreilly.com/admin` or `.review` for QA). Applies broadly, not just to feedback-service — reuse for future SOL/HD access-request tickets against GCP-hosted services.

## How it works
- Chassis services get a Django Admin via the `orm_admin` package, authenticated through **Groot** (O'Reilly's auth/authz service) via Unified Auth.
- On login, Groot tells the service what **Groups** the user belongs to; the service auto-creates matching Django Groups and assigns their permissions.
- `is_staff` (can log into admin at all) is synced from Groot automatically.
- `is_superuser` is NOT the same as Groot superuser — it only applies if the user's Groot group has the custom `orm_admin.is_superuser` permission in that specific service. By default the `Developers` group gets this.
- There is **no self-service group management** — someone with admin access to the target service's Django admin must create/configure the Group + permissions, then **`@guardian-devs` (Slack)** must be asked to add the requesting users to that Group in Groot so it syncs on next login.

## Steps to grant access
1. Identify the service's owning team (Cortex catalog `ownersV2.teams` — see [[reference-cortex-api]]) — they control what Groups/permissions exist in that service's admin.
2. Have the owning team confirm or create an appropriate Group (e.g. `<Service> Admin`) with the right Django permissions — avoid just dumping people into `Developers`.
3. Ask `@guardian-devs` in Slack to add the requesting users' O'Reilly emails to that Group in Groot.
4. Users log in via Unified Auth at the `/admin` URL — Groot syncs them into the Group on that login.

Source: `docs/guide/chassis/permissions.md` in the O'Reilly DevDocs clone (`~/src/devdocs/devdocs/docs/`).

## Example: SOL-106969 (2026-07-08)
Susanna Kline (ID team) requested feedback-service QA + Prod admin access for 4 people (cames, skline, cbremseth, dgonzalez @oreilly.com). Cortex shows feedback-service is owned by team **oreillymedia-interactivity** (Brian Glass, Christopher Henley, Diane Gleeson, Jason Kane, Kelvin Hammond, Marcel Cary, mirandaLmota — repo `oreillymedia/feedback-service`).

**Real-world process (per Scott Moschella in Slack `#vs-micro-feedback-and-diagnostic-collab`, 2026-07-08/09) — supersedes the generic `@guardian-devs` guess above for this service:**
1. Each user logs into the admin URL directly, clicks **"unified auth"**, enters their O'Reilly email, then chooses **"sign in with password"** — NOT SSO. (SSO button can error if already signed into the platform with a non-admin account.)
2. If after login they see a **blank dashboard** (no records visible), that means they're authenticated but have no Django permissions yet.
3. In that case, ping **Scott Moschella** in `#vs-micro-feedback-and-diagnostic-collab` (or presumably reach him directly) with the list of emails — he grants the permissions himself to view feedback records. No separate `@guardian-devs` step was needed in practice for this service.
4. QA feedback records specifically live at `https://feedback-service.platform.gcp.oreilly.review/admin/feedback_service/quizfeedback/`.
5. Note: **Slack notifications for new feedback only fire in Production**, not QA (confirmed by Scott Moschella).

**Why GitHub usernames sometimes show up on these requests (and can be ignored here):** O'Reilly's standard new-hire access template (`docs/guide/fe/access.md` in DevDocs) has managers email `solutions@oreilly.com` with a GitHub username to add people to the `oreillymedia`/`safarijv` GitHub orgs/teams (repo access, Jenkins, CDN) — a totally separate grant from Chassis service admin access. Reporters often include GH handles out of habit from that template even when the actual ask (like this one) only needs email, since Groot/Unified Auth admin login has no GitHub tie-in.

**Business context:** This access is for the ID (Instructional Design) team — Susanna Kline, Desi Gonzalez, Charlotte Ames, Chris Bremseth — to dry-run "question microfeedback" during pre-launch UAT: QA for internal AI Agents/Agentic Coding diagnostics testing, and Prod for UAT on hidden (URL-only) skill plans. Illana Stanley is doing e2e testing of quiz microfeedback in QA already; hasn't yet validated prod end-to-end.
