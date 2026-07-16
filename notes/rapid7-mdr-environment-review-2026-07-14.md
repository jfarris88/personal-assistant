# Rapid7 MDR Environment Review — 2026-07-14

**Meeting:** MDR Environment Review and Action Items
**Date/Time:** July 14, 2026, 10:39 AM PDT

## Summary
Reviewed ongoing MDR monitoring issues: agent coverage, legacy OS usage, event source ingestion errors, and cloud logging visibility. Key action items: verify asset agent status, address Mac and Linux agent errors, prioritize Google Cloud logging setup. Core event sources (DNS, DHCP, firewall) emphasized as critical for SOC visibility.

## Key Points
* No updates to contact list; no open cases except a recently closed Sophos event source issue pending Elton's confirmation
* Agent count dropped slightly to 769; need to verify if assets were decommissioned or replaced to address visibility gaps
* Many assets still run older agent versions on legacy OSs; some receive long-term security patches through licensing, but upgrades recommended for security hygiene
* Mac full disk access error persists on one device (Brad's); Linux audit compatibility errors remain on 11 devices; responsible teams identified for fixes
* Several core event sources (DNS, DHCP, firewall) have ingestion errors or have stopped sending data; Alon to follow up on status and cleanup
* Google Cloud logging is critical due to infrastructure presence; setup delayed mainly due to Brad's absence; logging costs and configuration complexity discussed
* MDR supports third-party triage for tools like Okta and Google SCC; enabling these improves SOC detection capabilities
* Custom and contextual detections require team review to tune alerts and reduce noise; SOC can assist with tuning or suppression

## Action Items
* Elton to confirm Sophos event source status and licensing issues
* Team to review agent management page for missing or stale agents and verify asset decommissioning or replacements
* Brad's Mac full disk access error to be resolved; Linux team to address audit compatibility errors on Linux devices
* Alon to verify and clean up non-ingesting event sources including DNS, DHCP, and firewall logs
* Follow up with Brad to prioritize Google Cloud logging setup and SCC event source configuration
* Team to regularly review custom detection alerts and coordinate with SOC for tuning or suppression

## Open Questions
* Are there any recent asset decommissions or replacements causing the agent count drop?
* What is the current status of the firewall replacement and removal of the old Cisco ASA event source?
* How extensive is the Google Cloud infrastructure currently, and what critical processes are hosted there?
* Is Salesforce threat detection actively used and should its event source be configured?
* Are there any additional questions or concerns about tuning custom and contextual detections?
