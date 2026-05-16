# Open Questions

Active onboarding questions for the human Salesforce admin
(`SLACK_APPROVAL_USER_ID`).

## 2026-05-16 - Org purpose and environment

- Question: Is `Walrus Holdings Pte. ltd.` (`00Dg5000003FriEEAS`,
  `orgfarm-dev`) a real production org, a demo / training workspace, or
  something else? Should agents treat it as production for safety?
- Why it matters: drives risk tolerance for any future write actions and how
  strictly to interpret validation rules.
- Observed evidence: Developer Edition, `IsSandbox=false`, fictional account
  names, only 7 / 7 / 5 / 0 / 0 / 13 record counts.
- Asked in Slack: yes
- Status: open

## 2026-05-16 - Second Salesforce org

- Question: What is the second authenticated org
  (`shubham.91664b5e4190@agentforce.com`, `00DgK00000LMHW8UAP`) used for?
  Should agents ever target it?
- Why it matters: avoid accidentally running queries or future writes
  against the wrong org.
- Observed evidence: `sf org list` returns two non-scratch orgs; neither is
  flagged as default.
- Asked in Slack: yes
- Status: open

## 2026-05-16 - Slackbot integration user

- Question: What is the `Slackbot` user
  (`slackbot@00dg5000003frieeas.ext`) wired to? Any connected app, named
  credential, or external integration we should know about?
- Why it matters: governs whether any data on Account / Opportunity / Case
  is owned by an external integration.
- Observed evidence: user has no profile, no role, has never logged in.
- Asked in Slack: yes
- Status: open

## 2026-05-16 - Opportunity StageName picklist mismatch

- Question: Are `Proposal`, `Negotiation`, `Closed Won` the official sales
  stages? Should the picklist be reduced to only those values, or is it
  intentionally non-restricted?
- Why it matters: filtering, forecasting, and any future automation needs to
  use the right vocabulary.
- Observed evidence: picklist metadata still exposes Salesforce defaults
  (`Prospecting`, `Qualification`, ..., `Negotiation/Review`,
  `Proposal/Price Quote`, `Closed Lost`). Actual records only use the short
  labels.
- Asked in Slack: yes
- Status: open

## 2026-05-16 - Opportunity Type picklist mismatch

- Question: Are `New Business`, `Expansion`, `Renewal` the official
  opportunity types? Should the picklist drop the Salesforce default values
  (`Existing Customer - Upgrade`, etc.)?
- Why it matters: `ARR_Field_Type_Match` only recognizes the short labels.
- Observed evidence: actual records use only the short labels; picklist
  still advertises defaults.
- Asked in Slack: yes
- Status: open

## 2026-05-16 - Account.Type and the `Churned` value

- Question: Should `Churned` be added to the `Account.Type` picklist? The
  `Account_Type_Forward_Only` validation rule already references it but no
  picklist value exists.
- Why it matters: the lifecycle rule cannot complete `Prospect -> Customer
  -> Churned` until the picklist value exists; today no record can ever be
  set to `Churned` without an Apex/API bypass.
- Asked in Slack: yes
- Status: open

## 2026-05-16 - `test_field__c` validation rule

- Question: The `test_field_must_be_gt_100` validation rule on Account
  refers to a field `test_field__c` that does not currently exist on
  Account. Should we delete or deactivate this rule?
- Why it matters: if anyone re-creates the field with the same name, or a
  data load includes that column, the rule will fire unexpectedly.
- Asked in Slack: yes
- Status: open

## 2026-05-16 - ARR_Legacy__c status

- Question: Is `ARR_Legacy__c` officially deprecated? Should agents avoid
  reading and writing it entirely, or is it still used for historical
  reporting?
- Why it matters: agents must not double-count ARR or pick the wrong field
  in reports / writes.
- Observed evidence: only one Closed Won opp populates it
  (`Summit Software - Upsell`, $30,000) with the same value as
  `ARR_Expansion__c`.
- Asked in Slack: yes
- Status: open

## 2026-05-16 - Sample custom fields on Account / Opportunity

- Question: Are the Developer Edition sample fields
  (`CustomerPriority__c`, `SLA__c`, `Active__c`, `UpsellOpportunity__c`,
  `NumberofLocations__c`, `SLASerialNumber__c`, `SLAExpirationDate__c`,
  `DeliveryInstallationStatus__c`, `TrackingNumber__c`, `OrderNumber__c`,
  `CurrentGenerators__c`, `MainCompetitors__c`) intentionally unused, or
  do you plan to use any of them?
- Why it matters: agents will otherwise either ignore them (correct) or
  recommend them in queries (incorrect).
- Asked in Slack: yes
- Status: open

## 2026-05-16 - Case routing and ownership

- Question: Cases have `Type` null on every record and no queues or
  assignment rules. Is manual ownership intentional? Is there a documented
  triage path for Critical cases (e.g., the security incident)?
- Why it matters: governs what an agent should suggest when a new case is
  created or when a Critical case appears.
- Asked in Slack: yes
- Status: open

## 2026-05-16 - Flow / automation gaps

- Question: Are there any active Flows, Process Builders, Workflow Rules,
  or Approval Processes the read-only Tooling API did not surface? If yes,
  please point at them so we can document side effects.
- Why it matters: the current automation pass found only validation rules
  and packaged Apex; missing flows will cause wrong agent guidance.
- Asked in Slack: yes
- Status: open

## 2026-05-16 - Reporting and KPI conventions

- Question: Are there report folders, dashboards, or KPIs the org expects
  agents to follow (e.g., ARR roll-ups, pipeline by Stage, Critical Cases
  open)?
- Why it matters: so agents recommend the right field combinations and
  filters instead of inventing them.
- Asked in Slack: yes
- Status: open
