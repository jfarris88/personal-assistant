# GCP Project Ownership

Quick reference for which team owns which GCP projects, based on Jira ticket history.

## Platform Engineering (`PE` project)
Owns and manages via Terraform:
- `platform-prod-399563`
- `platform-dev-788014`

PE handles all SA creation, deletion, and IAM changes in these projects. Engineers (including ITOPS) open PE tickets for any work here. Primary assignee: **Justin Breninger** (`jbreninger`). Route requests through **Shawn Storc** (`sstorc`), the PE PM.

## ITOPS (`ITOPS` project)
Owns and manages via `itops-infra-tf` Terraform repo:
- `data-prod-378016`
- `data-uat-1`
- `data-dev-378016`
- `data-qa-1`

ITOPS handles SA creation and IAM in these projects. Primary implementer: **Alain Mbuku** (`ambuku`). VP-level approval (Suganthi Senthil, VP Data) required for SA requests in data-prod per precedent in ITOPS-43896.

## Routing rule
If a service account request targets a `platform-*` project → PE ticket.
If it targets a `data-*` project → ITOPS ticket (with data org approval).
