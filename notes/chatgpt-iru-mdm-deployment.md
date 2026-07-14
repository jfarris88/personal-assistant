# ChatGPT Desktop App — Iru MDM Deployment (Windows)

**Date:** 2026-06-17
**Tags:** #mdm #iru #chatgpt #windows #powershell
**Confluence:** *(not yet created)*
**Jira:** [HD-37516](https://intranet.oreilly.com/jira/browse/HD-37516)

---

## Overview

ChatGPT was rolled out company-wide, but the ChatGPT auto app is not available in Iru's app catalog for Windows. A custom PowerShell script is needed to handle initial deployment and keep the app current.

## Action Items

- [ ] Write and test PowerShell deployment script
- [ ] Deploy via Iru as a custom script policy
- [ ] Validate auto-update behavior post-install
- [ ] Document in Confluence once complete

---

## Solution Approach

### How ChatGPT for Windows installs
OpenAI distributes the Windows desktop app as a user-space installer (Squirrel-based). Key behaviors:
- Installs **per-user** by default (into `%LOCALAPPDATA%\Programs\`), not system-wide
- Has a **built-in auto-updater** — once installed it will silently self-update in the background, so no ongoing script cadence is needed for updates
- The installer URL is stable: `https://persistent.oaistatic.com/sidekick/public/openai-setup.exe`

### Script strategy
Because it's a per-user install, the script should run in **user context** (not SYSTEM) in Iru. The script should:
1. Check if ChatGPT is already installed (skip if so)
2. Download the installer to a temp location
3. Run the installer silently
4. Clean up the temp file

### Draft PowerShell Script

```powershell
# Deploy-ChatGPT.ps1
# Run in USER context via Iru MDM

$installerUrl = "https://persistent.oaistatic.com/sidekick/public/openai-setup.exe"
$installerPath = "$env:TEMP\openai-setup.exe"
$installDir = "$env:LOCALAPPDATA\Programs\OpenAI"

# Check if already installed
if (Test-Path "$installDir\ChatGPT.exe") {
    Write-Output "ChatGPT already installed. Exiting."
    exit 0
}

# Download installer
Write-Output "Downloading ChatGPT installer..."
try {
    Invoke-WebRequest -Uri $installerUrl -OutFile $installerPath -UseBasicParsing
} catch {
    Write-Error "Download failed: $_"
    exit 1
}

# Silent install
Write-Output "Installing ChatGPT..."
Start-Process -FilePath $installerPath -ArgumentList "--silent" -Wait

# Cleanup
Remove-Item $installerPath -Force -ErrorAction SilentlyContinue

Write-Output "ChatGPT installation complete."
exit 0
```

### Auto-Update
No ongoing script is needed for updates. The Squirrel-based updater bundled with the app checks for and applies updates silently on launch. Verify this is working after initial rollout by checking the version on a test machine after a few days.

### Iru Deployment Notes
- Set script execution context to **User** (not System/Local Service)
- Target the Windows device group or a pilot group first
- Iru may require the script to be base64-encoded or signed depending on your policy settings — check your current Iru PowerShell policy
- If Iru supports detecting installed apps by path, set detection rule to: `%LOCALAPPDATA%\Programs\OpenAI\ChatGPT.exe`

---

## Blockers (2026-06-17)

- ❌ **No MSI or system-wide EXE exists** — ChatGPT on Windows is distributed exclusively via the Microsoft Store as an MSIX package. OpenAI does not publish a Win32 MSI or machine-wide EXE installer. (Researched 2026-06-17)
- ❌ **ChatGPT installs in user session, not system-wide** — Iru's Custom App feature requires an MSI or EXE and can't push user-session installers. (Confirmed by Elton Lee)
- ❌ **Iru detection rules don't support user-session installs** — previously hit this same wall with the Drata agent deployment. The detection mechanism expects system-level paths.
- ❌ **Intune not available** — the most widely documented enterprise deployment method (Intune "Microsoft Store app (new)" with System context) is not an option. We are Iru-only for MDM.

## Only Remaining Option Worth Exploring

**Winget via PowerShell script in Iru** — OpenAI documents this deployment command:
```
winget.exe install --id=9NT1R1C2HH7J --source=msstore --accept-package-agreements --accept-source-agreements --silent
```
This could be wrapped in a PowerShell script and pushed via Iru, but the user-session detection rule limitation likely still applies. Would need to confirm whether Iru has any mechanism to handle user-session app detection, or whether this requires a support request to Iru.

## Open Questions / Next Steps

- [ ] Does Iru have any mechanism for detecting user-session installs (workaround for the detection rule limitation)?
- [ ] Contact Iru support — is there a solution or roadmap item for user-session app deployment?
- [ ] How was the Drata agent situation ultimately resolved — is there a pattern we can reuse?

## References

- OpenAI ChatGPT desktop download: https://openai.com/chatgpt/download/
- Iru — Configure the Windows Custom App Library Item: https://docs.iru.com/en/endpoint/library/library-items-profiles/configure-the-windows-custom-app-library-item#configure-the-windows-custom-app-library-item
