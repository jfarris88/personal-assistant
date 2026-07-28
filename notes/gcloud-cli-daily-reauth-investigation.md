# gcloud CLI Daily Re-Auth Investigation (2026-07-23)

## Trigger
Kevin Graves asked in Slack (Helpdesk channel, 2026-07-23 9:35am) whether IT changed anything related to `gcloud` CLI auth — he and others now have to log in almost every day. Jay replied IT made no changes and would research and DM back.

## Findings (via Google Cloud/Workspace docs research, not O'Reilly-specific confirmation)

**Most likely cause: "Google Cloud session control" — a Workspace Admin console setting, not a gcloud/IT change.**
- Location: Admin console > Security > Access and data control > Google Cloud session control.
- Controls reauthentication frequency for: Cloud Console, the `gcloud` CLI, and any third-party/custom app using Google Cloud OAuth scopes. Does **not** apply to the Console mobile app.
- Configurable per-OU, range 1–24 hours, method password or security key.
- Key detail: **orgs created before 2023 often default to "never require reauthentication."** O'Reilly's Google Cloud org predates 2023, so if this setting was never touched, gcloud sessions should persist indefinitely. Daily forced logins strongly suggest **someone (a Workspace super admin — possibly outside Helpdesk, e.g. Security team) recently enabled or shortened this policy**, rather than Google silently changing a default.
- Source: [Set session length for Google Cloud services (Workspace Admin Help)](https://knowledge.workspace.google.com/admin/security/set-session-length-for-google-cloud-services), [Google Cloud reauthentication docs](https://docs.cloud.google.com/docs/authentication/reauthentication)

**Ruled out: Google's 2026 "reauthentication for sensitive actions" rollout.**
- This is a separate, newer Google-side feature (global rollout "expected complete in 2026") that forces password/MFA re-entry before sensitive actions (billing changes, IAM policy edits at org/folder/project level).
- It only fires for actions taken **in the Cloud Console UI**, not the `gcloud` CLI — so it doesn't explain Kevin's CLI experience.
- Source: [Reauthentication for sensitive actions](https://docs.cloud.google.com/docs/authentication/reauthentication)

## Verification still needed (requires Admin console access — not available to Claude in this session)
Check **Admin console > Reports > Audit and investigation > Admin log events**, filtered to "Google Cloud session control" setting changes, to confirm:
1. Whether the policy was recently changed (and by whom / which OU).
2. The current configured reauth frequency.

If nobody on the IT/Security side changed it intentionally, worth asking whether a broader security-hardening initiative (SOC2, Rapid7 findings, etc. — see [current_projects.md](../knowledge/current_projects.md) #13) enabled it org-wide without looping in Helpdesk.

## Suggested Slack reply to Kevin
> Dug into this — we didn't touch anything gcloud-related on our end, but there's a Google Workspace setting called "Google Cloud session control" (Admin console > Security > Access and data control) that controls how often `gcloud`/Console sessions require re-auth, separate from anything app-specific. Orgs as old as ours normally default to never forcing re-auth, so daily logins point to that org-wide policy having been changed/enabled recently — not a bug or something on your end. Checking our admin audit log now to see who/when, and whether it was intentional (could be part of a security hardening push). Will follow up once I know more.

## Update 2026-07-23 — org policy ruled out, issue is wider than Kevin

Jay checked Admin console directly (screenshots): **Google Cloud console and SDK session control = "Never require reauthentication"**; separate **Google session control (web) = 14 days**. Neither explains daily forced logins. Kevin independently confirmed the same in Slack ("CLI is set to never log out").

**Kevin posted in `#engineering-general`** (wider thread, not just his AM-2443 IAM-group group): multiple engineers confirm the same symptom, and it's not limited to `gcloud` itself:
- **Cris Pope** — team hitting it almost daily.
- **Kenneth Love** — same for `orm setup pypi --auth-only`.
- **Dave Carroll** — initially guessed org policy (now ruled out by both Jay and Kevin).
- **Bill Hilbert** — goes through browser auth flow, lands on a blank "Ok" page, then nothing works.
- **Daniel Daly** — killing and restarting `local-platform` fixed it for him.
- **Curtis Smith** — workaround: `gcloud auth login --no-launch-browser` (manual code paste).
- **Kevin, 10:46am** — "No, not based on the current config, something else is up."

**Key finding: the common thread is Application Default Credentials (ADC), not plain `gcloud auth login`.**
- `orm setup pypi --auth-only` auths GAR PyPI via the `keyrings.google-artifactregistry-auth` keyring package, which rides on `gcloud auth application-default login` — confirmed via [orm-cli README](https://devdocs.common-build.gcp.oreilly.com) (`~/src/devdocs/devdocs/docs/orm-cli/README.md`), not the interactive gcloud user session.
- Kevin's BigQuery MCP work ([AM-2443](https://intranet.oreilly.com/jira/browse/AM-2443)) also depends on ADC.
- So multiple superficially-different tools failing the same way points at ADC token handling specifically, not "gcloud" as a whole.

**Bill's "blank Ok page, nothing works" + Daniel's local-platform fix** suggest an environmental/local cause for at least that variant: `local-platform` (`~/src/devdocs/devdocs/docs/local-platform/README.md`) runs a local Traefik proxy with custom `/etc/hosts` entries and its own port bindings — plausible interference with the loopback HTTP listener `gcloud auth login` opens to catch the OAuth redirect, independent of the daily-reauth complaint itself.

**No public corroboration found** — searched Google Cloud status dashboard, release notes, and developer forums for a matching 2026 incident/bug; nothing conclusive. Can't confirm or rule out a Google-side change to ADC token risk/revocation behavior.

**Recommendation:** this has outgrown a Helpdesk-only answer — it's a cross-team engineering tooling issue (orm-cli, local-platform, BigQuery MCP all implicated). Worth looping in Justin Breninger (Platform Engineering — owns GCP infra + orm-cli) rather than Jay chasing it solo.

## Update 2026-07-23 (later) — confirmed Google-side bug, not O'Reilly-caused

**Justin Breninger** posted in the `#engineering-general` thread (per Jay, 11:10am) identifying this as a **Google-side bug**, not an O'Reilly config/policy issue — matches the direction this investigation was already heading (org policy ruled out, ADA/ADC common thread across tools).

Jay surfaced two supporting links:
- Reddit: [r/googlecloud — "Issue this week with gcloud CLI auth tokens"](https://www.reddit.com/r/googlecloud/comments/1v3aths/issue_this_week_with_gcloud_cli_auth_tokens/) — other orgs reporting the same thing this week, suggesting a broader/external issue rather than something O'Reilly-specific.
- Google Issue Tracker: [issuetracker.google.com/issues/537030491](https://issuetracker.google.com/issues/537030491) — presumably the tracked bug Justin matched to.

**Not independently verified by Claude** — both links require authentication/access Claude doesn't have (Issue Tracker needs Google sign-in; Reddit blocks fetch tools outright, and no browser session was available to work around it). Content/specifics should be captured from Jay/Justin directly rather than assumed.

Jay's read (11:27am): "I think it's a bug on their side" — echoing Justin, and flagging concern about whether Google intends this behavior or will fix it.

## Update 2026-07-23 (later still) — it's a rollout, not a bug; ticket filed; fix path identified

Kevin filed **[AM-2475](https://intranet.oreilly.com/jira/browse/AM-2475)** ("Requesting configuration changes for Google policy on CLI logouts") once the real cause became clear: this is **not a bug** — it's Google's long-running (started 2023, completing through 2026) rollout that force-migrates **all** orgs to a mandatory **16-hour default session length** for Cloud Console + `gcloud` CLI + any app using Cloud OAuth scopes. Org policy currently shows "Never require reauthentication" in the Workspace Admin console, but that's about to become unenforceable — Google overrides it silently as the rollout reaches O'Reilly's org, regardless of what the console UI displays.

**Correct fix mechanism (per Google's own Access Context Manager docs, more precise than the Workspace Admin console UI toggle, and supersedes it):**
1. Create a Google Group for users who need the extended/exempt session (e.g. an engineering group).
2. Create a **Cloud Access Binding** (`gcloud access-context-manager cloud-bindings create --organization=ORG_ID --group-key=GROUP_ID --binding-file=binding.yaml`) scoping a custom session policy to just the "Google Cloud SDK" client app for that group — either `sessionLengthEnabled: false` (full exemption) or a long `sessionLength` (e.g. 86400s/24h).
3. **Requires `roles/accesscontextmanager.gcpAccessBindings.create` at the GCP Organization level** — a step above typical Helpdesk/access-management scope; needs whoever administers the root `oreilly.com` GCP Org node.
4. Gotcha: only the most-recently-created matching binding applies per user — a later, unrelated binding for an overlapping group would silently supersede this fix.
Full detail/sources: [Configure session controls for reauthentication](https://docs.cloud.google.com/access-context-manager/docs/session-controls-for-reauthentication) (see `#gcloud` and example-policy-configuration sections), [Groups in Cloud console](https://docs.cloud.google.com/iam/docs/groups-in-cloud-console).

**Assignee problem (2026-07-23):** Justin Breninger — who surfaced the Google issue tracker match and was the obvious PE candidate to execute this — **has transferred out of Platform Engineering**. He is not the right assignee. Need to identify who now holds org-level GCP IAM/Access Context Manager admin rights before AM-2475 can move forward. See [key_people.md](../knowledge/key_people.md) update.

## Status
Root cause confirmed: **Google's 2026 mandatory session-length rollout**, not a bug, not an O'Reilly-caused config issue. Fix mechanism identified (Cloud Access Binding via Access Context Manager, scoped to a group). **Blocked:** waiting to identify Justin Breninger's replacement / whoever now holds org-level Access Context Manager admin rights, before drafting the AM-2475 comment with next steps. Jay asked to hold off on posting the comment until that's sorted.
