# CDN Access — Setup, Usage, and Approval Process

**Last updated:** 2026-07-09  
**Sources:** AM-1041, AM-1482, AM-1498, AM-1500, AM-1573, AM-2409, ITOPS-44883, SOL-23324, SOL-23627, Confluence MKT space

---

## What is the CDN?

O'Reilly's CDN is backed by **Akamai** and served from `cdn.oreillystatic.com`. Files are hosted on a server at `cdn-access.corp.oreilly.com` and uploaded via SFTP. Content placed there is publicly accessible — no paywall.

Common uses: cover images, topic hub pages, podcast assets, logos, course materials, badge images, radar images.

---

## Requesting Access

### Where to file
File in the **AM project** as an **Access Request** issue type. (Tickets sometimes come in via SOL and get converted/duplicated into AM — either is fine, but AM is the correct home.)

### Who approves
IT approves CDN access requests. There is no required manager sign-off based on historical precedent:

- **Jay Farris** approved AM-1041 (Jun 2024) and AM-1482 (May 2025)
- **Brian Crebs** approved AM-1498 (May 2025) — directed AM team to add users to AD group
- AM-1500 (May 2025) — Olivia MacDonald (team manager) self-approved for her own team
- AM-1573 (Jun 2025) — Shiny Kalapurakkel (manager) requested for a direct report, no separate approver needed

**In practice:** IT (Jay Farris or Brian Crebs) is the approver. For VPs or senior staff requesting on their own behalf, their title is sufficient authorization — no escalation required.

### What to include in the request
- Who needs access (name + email)
- What directory/path they'll be writing to
- Brief business justification

---

## Provisioning Access (IT Steps)

1. **Add the user to the AD group:** `gcp-cdn-origin-users` (also referred to as `gcp-cdn-origin-usergroup`)
   - This is done via Active Directory / Okta
2. **Have the user add an SSH public key to their Okta settings**
   - After the overnight AD sync, SFTP access will work
3. **Optionally create a dedicated subdirectory** under `/title/cdn-origin/htdocs/` for the team/use case (see AM-1482: `oreilly/helpdesk/` was created for Helpdesk)

The old process (pre-GCP migration) used password-based SFTP with the user's email password. Current process uses SSH key auth via Okta.

---

## Connecting to the CDN (End User Instructions)

### Requirements
- Must be on the **O'Reilly VPN**
- Must have an **SSH public key** added to your Okta profile

### Option 1: Cyberduck (recommended GUI client)

1. Open **Cyberduck** and click the **+** icon (bottom left) to create a new bookmark
2. Use these settings:

| Field | Value |
|---|---|
| Protocol | SFTP (SSH File Transfer Protocol) |
| Server | `cdn-access.corp.oreilly.com` |
| Username | Your short username (e.g. `jsmith`, not `jsmith@oreilly.com`) |
| SSH Private Key | `~/.ssh/id_rsa` |
| Remote Path | The directory you've been granted access to |

3. Double-click the bookmark to connect
4. Drag and drop files to upload

### Option 2: Terminal / SFTP

```bash
ssh cdn-access.corp.oreilly.com
```

Or via sftp:

```bash
sftp username@cdn-access.corp.oreilly.com
```

### Option 3: Any SFTP client (Transmit, Fetch, etc.)

Same settings as Cyberduck above.

---

## File Paths and Public URLs

The CDN filesystem root maps to `https://cdn.oreillystatic.com/`:

| Filesystem path | Public URL |
|---|---|
| `/usr/local/www/downloads/` | `https://cdn.oreillystatic.com/` |
| `/title/cdn-origin/htdocs/` | `https://cdn.oreillystatic.com/` |
| `/title/cdn-origin/htdocs/oreilly/images/` | `https://cdn.oreillystatic.com/oreilly/images/` |
| `/title/cdn-origin/htdocs/assets/course-materials/` | `https://cdn.oreillystatic.com/assets/course-materials/` |
| `/title/cdn-origin/htdocs/oreilly/helpdesk/` | `https://cdn.oreillystatic.com/oreilly/helpdesk/` |

**Example:** Upload `logo.png` to `/title/cdn-origin/htdocs/oreilly/helpdesk/` → publicly visible at `https://cdn.oreillystatic.com/oreilly/helpdesk/logo.png`

---

## Purging the Cache (After Replacing a File)

If you replace an existing file on the CDN, you must purge the Akamai cache or the old version will continue to be served.

### Option 1: Web portal (easiest)

1. Go to `https://admin.oreilly.com/akamai/purge_new.csp`
2. Paste the fully-qualified public URL of the file (e.g. `https://cdn.oreillystatic.com/oreilly/images/logo.png`)
3. Click **Submit** — purge completes in ~5 seconds (up to 5 minutes in rare cases)

### Option 2: Command line

```bash
# SSH into the CDN server first
ssh cdn-access.corp.oreilly.com

# Then run the purge command
aka_purge.py https://cdn.oreillystatic.com/oreilly/images/logo.png
```

Note: You only need to purge once — purging either `http://` or `https://` covers both.

If the purge doesn't take effect within 10 minutes, try again. If repeated purges fail, contact solutions@oreilly.com.

---

## Relevant Confluence Pages

- [CDN Connect, Upload, and Cache Purge](https://intranet.oreilly.com/confluence/pages/viewpage.action?pageId=31130488) — original MKT space doc (2013, still accurate for connection basics)
- [CDN access for Instructional Design](https://intranet.oreilly.com/confluence/pages/viewpage.action?pageId=146639805) — updated 2025 doc with Cyberduck screenshots
- [Adding SSH keys for Linux system authentication](https://intranet.oreilly.com/confluence/pages/viewpage.action?pageId=79135041) — space IS; how to add the SSH public key to your Okta profile required for CDN SFTP access. Flagged as a high-importance doc for the [Confluence Cloud migration](confluence-cloud-migration-candidates.md).

## Related Jira Tickets (reference)

- [AM-1041](https://intranet.oreilly.com/jira/browse/AM-1041) — Daniel Daly, Jun 2024
- [AM-1482](https://intranet.oreilly.com/jira/browse/AM-1482) — Justin Breninger, May 2025
- [AM-1498](https://intranet.oreilly.com/jira/browse/AM-1498) — Andy Seronick, May 2025
- [AM-1500](https://intranet.oreilly.com/jira/browse/AM-1500) — Chris Bremseth + team, May 2025
- [AM-1573](https://intranet.oreilly.com/jira/browse/AM-1573) — Peyton Joyce, Jun 2025
- [AM-2409](https://intranet.oreilly.com/jira/browse/AM-2409) — Ryan Daly, Jun 2026 (Approved; Dean Roman requested Chris Stone train Ryan before access is granted)
