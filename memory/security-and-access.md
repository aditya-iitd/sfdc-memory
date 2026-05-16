# Security And Access

Status: initial discovery from read-only inspection on 2026-05-16. Confidence
medium unless noted.

## Active and inactive users (13 rows)

System / platform users:

- `Integration User` (Analytics Cloud Integration User profile)
- `Automated Process`
- `Platform Integration User`
- `Data.com Clean`
- `Chatter Expert` (Chatter Free User)
- `Security User` (Analytics Cloud Security User)
- `Slackbot` (`slackbot@00dg5000003frieeas.ext`, no profile/role,
  `IsActive=true`, never logged in). Likely a placeholder user for a Slack
  integration; purpose not confirmed - admin question logged.

Admin / business users:

- `OrgFarm EPIC` (`epic.c43ba7ed76d9@orgfarm.salesforce.com`,
  System Administrator). Org Farm provisioning user; last login
  2026-02-15.
- `Shubham Goel` (`shubham.c5391323dee2@agentforce.com`,
  System Administrator, UserRole `VP Sales`). Email
  `shubham@walrus.holdings`. Most recent login 2026-05-16. Treat as the
  primary working admin / owner of this org pending admin confirmation.
- `Sarah Chen` (`sarah.chen@agentforce.hiring.test`, Standard User,
  UserRole `Sales Manager`). Active, never logged in.
- `John Smith` (`john.smith@agentforce.hiring.test`, Standard User,
  UserRole `Account Executive`). Active, never logged in.

Inactive:

- `Jordan Jordan` (`jorddddddan@example.com`, Standard User). Deactivated.
- `Aditya User` (`aditya.20260516@walrusholdings.demo`, Standard Platform
  User). Deactivated. Created 2026-05-16 - likely a test of a provisioning
  flow.

## Roles

Selected UserRoles observed (subset; standard hierarchy template):

- `CEO`, `CFO`, `COO`
- `VP Sales` (held by Shubham Goel)
- `Sales Manager` (held by Sarah Chen)
- `Account Executive` (held by John Smith)
- `Channel Sales Team`, `Director, Channel Sales`
- `Customer Support, North America`, `Customer Support, International`

The full role hierarchy was not mapped. Most roles look like the Salesforce
template hierarchy; only three roles are actually populated by humans.

## Profiles in use

- `System Administrator` - Shubham Goel, OrgFarm EPIC.
- `Standard User` - Sarah Chen, John Smith, Jordan Jordan (inactive).
- `Standard Platform User` - Aditya User (inactive).
- `Analytics Cloud Integration User`, `Analytics Cloud Security User`,
  `Chatter Free User` - platform integration profiles only.
- Many additional standard profiles exist but are unassigned.

## Custom permission sets

Custom permission sets present (`IsCustom = true`):

- `API_Access`
- `AgentforceServiceAgentUserPsg`
- `Commerce_Shopper`
- `CopilotSalesforceAdminPSG`, `CopilotSalesforceUserPSG`
- `Developer_Edition`
- `Enablement_Admin`, `Enablement_Resources_Manager`, `Enablement_User`,
  `Partner_Enablement_User`
- `Experience_Profile_Manager`
- `Forecast_Editor`
- 11 `SubscriptionManagement*` permission sets
- A handful of `X00e...` and `00e1P...` permission sets with synthetic
  names - these look like temporary or session-scoped sets; risk of being
  mis-targeted is low because they have non-user-facing names, but admin
  should confirm whether any can be cleaned up.
- `sfdc_nc_constraints_engine_deploy` (Constraint Rules Engine licenseless
  permission set)
- `sfdc_scrt2` (SCRT2 Integration User)

Permission set ASSIGNMENTS were not enumerated yet (next discovery cycle).

## Queues and assignment rules

- No `Group` rows of type `Queue` were returned.
- No Case `AssignmentRule` rows are queryable via standard SOQL in this org;
  Tooling API does not appear to expose any custom assignment-rule metadata
  here. Confidence medium that there are no active assignment rules driving
  Case routing - admin can confirm.

## Sharing and ownership signals

- Account ownership is split between Sarah Chen (5 accounts) and John Smith
  (2 accounts). Confidence high.
- Opportunity ownership follows account ownership in every observed case.
  Treat the account owner as the source of truth for sales follow-up unless
  the admin says otherwise.
- The `VP Sales` role (Shubham Goel) sits above Sales Manager (Sarah) and
  Account Executive (John) by name; whether the actual hierarchy has those
  parent links is not verified.

## Sensitive data

- No PII has been read into memory. The org has no Lead / Contact records
  and very limited Case content.
- The Slackbot integration user and the unknown second org
  (`00DgK00000LMHW8UAP`) are the two largest unknowns. Treat any
  integration that runs as Slackbot as sensitive until the admin confirms
  scope.

## Caveats

- Profile-level CRUD/FLS, sharing rules, role hierarchy parents, and
  permission set assignments were not yet inspected. Treat security
  assertions as preliminary until a deeper pass is run.
