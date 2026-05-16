# Salesforce Instance

This repo tracks one Salesforce instance for now.

## Verification Rule

All facts below were observed with Salesforce CLI on 2026-05-16 and may change.
Before using any fact operationally, re-query Salesforce.

## Known Facts

- CLI alias used for this memory: `orgfarm-dev`
- Org ID: `00Dg5000003FriEEAS`
- Org name: `Walrus Holdings Pte. ltd.`
- My Domain / instance URL: `https://orgfarm-16e9a62082-dev-ed.develop.my.salesforce.com`
- Salesforce instance: `IND168`
- Environment: Developer Edition, not a sandbox (`IsSandbox = false`)
- API version observed: 66.0
- Org locale/language: `en_US`
- Org timezone: `America/Los_Angeles`
- Namespace prefix: none on the org
- Primary admin/contact inferred from active admin user: Shubham Goel
- Current CLI user observed: `shubham.c5391323dee2@agentforce.com`

## Connection Notes

- The Salesforce CLI had no configured default `target-org` when inspected.
- Two orgs were authenticated locally; `orgfarm-dev` was chosen because it was
  the only aliased org.
- Never store CLI access tokens or session IDs in this repo.
- Use `sf ... --target-org orgfarm-dev` unless the target org is changed by the
  user.

## Active Human And System Users Observed

Verify user status, role, profile, and assignments before changing access.

| User | Username | Profile | Role | Notes |
| --- | --- | --- | --- | --- |
| Shubham Goel | `shubham.c5391323dee2@agentforce.com` | System Administrator | VP Sales | Current CLI user and likely admin owner |
| OrgFarm EPIC | `epic.c43ba7ed76d9@orgfarm.salesforce.com` | System Administrator | none | OrgFarm/system admin user |
| Sarah Chen | `sarah.chen@agentforce.hiring.test` | Standard User | Sales Manager | Owns many records; has Forecast Editor permission set |
| John Smith | `john.smith@agentforce.hiring.test` | Standard User | Account Executive | Owns records; has API Access permission set |
| Integration User | `integration@00dg5000003frieeas.com` | Analytics Cloud Integration User | none | Active integration-style user |
| Security User | `insightssecurity@00dg5000003frieeas.com` | Analytics Cloud Security User | none | Active security-style user |
| Slackbot | `slackbot@00dg5000003frieeas.ext` | none | none | AutomatedProcess user, active, no last login |

## Open Questions

- Is `orgfarm-dev` still the intended target org?
- What Salesforce library or MCP server will the deployed agent use?
- Should user and customer record names be treated as demo data or real customer data?
