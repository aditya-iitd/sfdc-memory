# Permissions

How access should be handled in this Salesforce org.

## Verification Rule

All permission facts below were observed on 2026-05-16. User access changes are
volatile. Before answering or changing access, re-query users, profiles,
permission set assignments, object permissions, field permissions, sharing
settings, and license availability.

## Current Permission Model

- Profiles strategy: active business users currently use `Standard User` or
  `System Administrator`; standard Salesforce profile set is present, plus
  custom profiles named `Custom: Sales Profile`, `Custom: Marketing Profile`,
  and `Custom: Support Profile`.
- Permission sets strategy: a few regular permission sets exist and appear to
  carry specific exceptions.
- Permission set groups: 19 groups exist, mostly standard Agentforce,
  Enablement, Commerce, and Subscription Management groups. No custom business
  access bundle was observed.
- Role hierarchy: present. Active standard users observed in `VP Sales`,
  `Sales Manager`, and `Account Executive`.
- Sharing model for core objects:
  - Account: internal ReadWrite, external Private
  - Contact: internal ReadWrite, external Private
  - Opportunity: internal ReadWrite, external Private
  - Case: internal ReadWriteTransfer, external Private
  - Lead: internal ReadWriteTransfer, external Private
  - User: internal Read, external Private
- Public groups or queues: no `Group` rows of type `Queue` or `Regular` were
  observed; no queue/object mappings were found.
- Sharing rules: no SharingCriteriaRule, SharingOwnerRule, or RestrictionRule
  metadata found.

## How To Handle Permission Requests

When the agent is asked to grant or debug access:

1. Identify the exact access gap: object, field, record, app, tab, report,
   dashboard, flow, Apex class, connected app, or feature.
2. Check whether access is controlled by profile, permission set, permission set
   group, role hierarchy, sharing rule, team, queue, territory, or manual share.
3. Follow the org convention recorded below.
4. Prefer the narrowest durable access change that solves the request.
5. Record any new convention or exception in this file.
6. Check license availability before creating or activating users. The
   Salesforce license was fully used when inspected.

## Permission Conventions

- `API_Access` permission set is security-sensitive. Its description says:
  grants Salesforce API access for system integrations; must not be
  automatically assigned or copied when provisioning new users; requires
  explicit security review.
- `Forecast_Editor` permission set is intended for Sales Manager role and above.
  Its description says it grants ability to edit `ForecastCategory` on
  Opportunities.
- Verify the actual permissions inside each permission set before relying on
  labels or descriptions. SOQL did not show ObjectPermissions or FieldPermissions
  rows for `API_Access` or `Forecast_Editor` during inspection.

## Common Access Patterns

| Request Type | Where To Check | Preferred Fix |
| --- | --- | --- |
| Cannot see object | Profile/permission-set object permissions | Verify profile first; add narrow permission set only if this org convention is confirmed |
| Cannot see field | Field-level security and describe output | Verify field exists, queryability, and FLS; be careful with `Discount__c` |
| Cannot see record | OWD, owner, role hierarchy, shares | Core objects are internally broad, so record issues may be ownership, inactive user, or object-specific sharing |
| Cannot edit forecast category | `Forecast_Editor`, role | Intended for Sales Manager and above; currently assigned to Sarah Chen |
| Needs API access | `API_Access`, integration/security review | Do not copy automatically; verify security review and actual permissions |
| Cannot run report | Folder access, report type, object/field access | Unknown; inspect live |
| Cannot use flow or feature | Flow access, custom permission, permission set, Apex access | No flows were found, but inspect live |

## Known Permission Sets

| Permission Set | Label | Observed Assignment | Notes |
| --- | --- | --- | --- |
| `API_Access` | API Access | John Smith | Security-sensitive; do not auto-assign or copy |
| `Forecast_Editor` | Forecast Editor | Sarah Chen | Intended for Sales Manager role and above |
| `Developer_Edition` | Developer Edition | OrgFarm EPIC | Developer Edition app access |
| `Experience_Profile_Manager` | Experience Profile Manager | OrgFarm EPIC | Standard/admin capability; verify before use |

## Known Profiles

| Profile | Active Users Observed | Notes |
| --- | --- | --- |
| System Administrator | Shubham Goel, OrgFarm EPIC | Admin users |
| Standard User | Sarah Chen, John Smith | Business users |
| Analytics Cloud Integration User | Integration User | System/integration profile |
| Analytics Cloud Security User | Security User | Security/analytics profile |
| Chatter Free User | Chatter Expert | Standard generated user |
| none | Automated/process users | Platform Integration User, Automated Process, Slackbot, Data.com Clean |

## License Signals

- Salesforce license: 4 total, 4 used.
- Salesforce Integration license: 1 total, 0 used.
- Cloud Integration User license: 1 total, 1 used.
- Analytics Cloud Integration User license: 2 total, 2 used.
- Chatter Free license: 5000 total, 1 used.
- Customer/partner/community licenses exist but mostly unused.
- Agent behavior: always re-check `UserLicense` before creating or activating
  users.

## Open Questions

- Are there standard access bundles for teams or roles?
- Should this org prefer permission sets over profile edits for all new access?
- Who should review security-sensitive permissions like `API_Access`?
