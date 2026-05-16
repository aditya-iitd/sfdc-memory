# Automation

Flows, triggers, jobs, and automation side effects in this Salesforce org.

## Verification Rule

Automation changes quickly. Facts below were observed on 2026-05-16. Before
answering or making changes, re-check Flow metadata, Apex triggers/classes,
scheduled jobs, duplicate rules, and validation rules.

## Known Automation

| Name | Type | Object or Scope | Purpose | Notes |
| --- | --- | --- | --- | --- |
| Account validation rules | ValidationRule | Account | Enforce account lifecycle and strategic flag permanence | See `memory/policies.md` |
| Opportunity validation rules | ValidationRule | Opportunity | Enforce closed-stage immutability, minimum amounts, ARR field usage, and discount rules | See `memory/policies.md` |
| Standard duplicate rules | DuplicateRule | Account, Contact, Lead | Standard duplicate detection | Active for Account, Contact, Lead |
| Metalytics Data Loader Job for Org : 00Dg5000003FriE | Scheduled job | Org/data loader | Unknown | State WAITING; triggered 128 times when inspected |
| Program Milestone Computation Cron Job | Scheduled job | Program milestones | Unknown | State WAITING; triggered 510 times when inspected |
| Program Status Update Cron Job | Scheduled job | Program status | Unknown | State WAITING; triggered 255 times when inspected |

## Side Effects To Remember

- No Flow metadata was found by `sf org list metadata --metadata-type Flow`.
- No Apex triggers were returned by Tooling API.
- Four Apex classes exist from namespace `devedapp`: `DeveloperEditionUtils`,
  `DeveloperEditionUtilsTest`, `PostInstallScript`, and `PostInstallScriptTest`.
  These appear tied to the installed Developer Edition package, not custom org
  automation.
- Validation rules are the main custom automation/policy mechanism observed.
- Scheduled jobs exist and may modify program or data-loader related records;
  inspect before changing objects they touch.

## Open Questions

- What do the Metalytics and Program scheduled jobs actually modify?
- Are there hidden/managed flows not visible to the current CLI user?
- Should validation-rule policy be treated as production policy or demo/test
  scenario data?
