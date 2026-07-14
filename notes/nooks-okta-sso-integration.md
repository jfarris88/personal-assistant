# Nooks → Okta SSO Integration

## Goal
Connect O'Reilly's Nooks account to Okta for SSO. Procurement email `procurement-nooks@oreilly.com` is the IT admin account.

## Status (as of 2026-06-22)
- Heather Polzin set up `procurement-nooks@oreilly.com` as admin account (removed Thomas's bullpen access to free up a license)
- Billing: upfront (not monthly). Invoices should route to **Jamey Harvey** — he is currently not receiving them; Heather to investigate
- Nooks app added in Okta (OIDC from app catalog) — Client ID and Secret in hand
- Intro email sent to James Prayitno (2026-06-22) — asked to confirm secure method for sharing credentials, and inquired whether SAML is available as an alternative to OIDC
- **Waiting on response from James Prayitno before sharing credentials**

## Nooks SSO Setup Guide
https://nooks.help.usepylon.com/articles/2495765352-okta-sso-integration

**Protocol:** OIDC (only option in Okta app catalog — SAML availability TBD pending James's response)  
**Supported flow:** SP-initiated SSO (user lands on Nooks login page)

## Credentials to Send (once James responds)
- Client ID
- Client Secret
- Okta domain (e.g. `oreilly.okta.com`)
- Company email domain: `oreilly.com`
- Admin account: `procurement-nooks@oreilly.com`

## Contacts
- **Heather Polzin** — Nooks internal admin/owner at O'Reilly
- **James Prayitno** (`james.prayitno@nooks.in`) — Nooks AE/CSM

## Open Items
- [ ] Await response from James Prayitno re: secure credential handoff and SAML availability
- [ ] Send credentials once James confirms
- [ ] Resolve billing routing to Jamey Harvey
- [ ] Confirm license situation (Heather was checking on free IT admin seat)
