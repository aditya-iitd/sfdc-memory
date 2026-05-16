# Changelog

## 2026-05-16

- Added: initial org discovery from read-only `sf` CLI inspection of the
  `orgfarm-dev` org (`Walrus Holdings Pte. ltd.`, `00Dg5000003FriEEAS`).
- Added: data-model notes for Account / Opportunity / Case including the
  custom fields actually in use, plus a Deprecated Fields section covering
  `ARR_Legacy__c`, Developer Edition sample fields on Account and
  Opportunity, and picklist mismatches on `Opportunity.StageName`,
  `Opportunity.Type`, and `Account.Type`.
- Added: catalog of validation rules and noted the broken
  `test_field_must_be_gt_100` rule and the `Account_Type_Forward_Only`
  rule that references a missing `Churned` picklist value.
- Added: roster of active users, profiles, custom permission sets, and
  the unexplained `Slackbot` integration user.
- Added: business-process inferences (sales stages in use, ARR
  conventions, discount cap, account tiering, case routing observations).
- Added: agent-derived default policies pending admin confirmation.
- Added: 12 open questions in `memory/open-questions.md` covering org
  purpose, the second authenticated org, Slackbot integration, picklist
  mismatches, deprecated fields, validation-rule cleanup, automation
  visibility, case routing, and reporting conventions.
- Evidence: `sf org list`, `sf org display`, `sf org limits list`,
  Organization SOQL, `sf data query` aggregates on Account / Opportunity /
  Case / User, `sf sobject describe` for Account and Opportunity, Tooling
  API queries for `ValidationRule`, `ApexClass`, `ApexTrigger`,
  `CustomObject`, `InstalledSubscriberPackage`, `PermissionSet`,
  `UserRole`, `Profile`.


## 2026-05-16 (cycle 2)

- Added: report/dashboard inventory note (all packaged scaffolding, no
  org-authored business reports).
- Added: custom permission set assignments for John Smith (`API_Access`),
  Sarah Chen (`Forecast_Editor`), and OrgFarm EPIC (`Developer_Edition`,
  `Experience_Profile_Manager`).
- Evidence: SOQL on `Report`, `Dashboard`, `Folder`,
  `PermissionSetAssignment` filtered to custom permission sets.
- No admin answers received yet on the 12 open questions.


## 2026-05-16 (cycle 3)

- Added: activity pattern note. All Account / Opportunity / Case rows were
  created and last modified by Shubham Goel in the same ~10-second window on
  2026-05-16 11:33 UTC. Strongly suggests a demo data load, not live CRM
  activity.
- Added: surfaced previously-missed Case 00001121 - Security review - API
  token rotation needed, owned by Shubham Goel.
- Confirmed: no custom settings (IsCustomSetting = true returned 0 rows).
  Custom tabs all belong to the devedapp package; no org-authored tabs.
- Evidence: SOQL on Account / Opportunity / Case with LastModifiedDate and
  LastModifiedBy.Name; Tooling API on EntityDefinition (IsCustomSetting =
  true) and CustomTab.


## 2026-05-16 (cycle 4 - admin answers)

- Resolved: all 12 open onboarding questions. Admin (Shubham Goel via
  Slack thread) confirmed:
  - orgfarm-dev should be treated as production for safety.
  - The second authenticated org (00DgK00000LMHW8UAP) must be ignored.
  - For questions 3-12, the agent was authorized to assume based on
    read-only exploration.
- Updated: memory/open-questions.md now records resolutions and the
  agent-derived assumptions in detail.
- Updated: memory/policies.md now reflects production-grade handling
  rules, the ignore-instruction for the second org, and the inferred field
  / case / reporting defaults.
- Evidence: Slack thread message 1778956589.214189.
