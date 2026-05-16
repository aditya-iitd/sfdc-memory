# Open Questions

Active onboarding questions for the human Salesforce admin
(SLACK_APPROVAL_USER_ID).

All initial onboarding questions were resolved on 2026-05-16 via Slack
thread. Admin answers: (1) yes, treat orgfarm-dev as production for safety;
(2) ignore the second authenticated org; for items 3-12 admin authorized the
agent to assume based on read-only exploration. Those assumptions are now
recorded as agent-derived defaults in the relevant memory files.

## 2026-05-16 - Org purpose and environment

- Question: Is orgfarm-dev (Walrus Holdings) production / demo / something
  else, and should agents treat it as production for safety?
- Status: resolved
- Resolution: yes - agents must treat it as production for safety. Source:
  admin reply in Slack thread on 2026-05-16.

## 2026-05-16 - Second Salesforce org

- Question: What is org 00DgK00000LMHW8UAP used for?
- Status: resolved
- Resolution: ignore that org. Agents must not target it. Source: admin
  reply in Slack thread on 2026-05-16.

## 2026-05-16 - Slackbot integration user

- Question: What is the Slackbot user wired to?
- Status: resolved by admin permission to assume
- Resolution (agent inference): no integration is currently wired up that
  this read-only inspection can see. The Slackbot user has no profile, no
  role, has never logged in, and the org has no connected apps or named
  credentials. Treat the user as an unconfigured placeholder. Do not assume
  any Slack <-> Salesforce data sync exists. Re-confirm before relying on
  any Slack integration behavior. Confidence medium.

## 2026-05-16 - Opportunity StageName picklist mismatch

- Question: Are Proposal, Negotiation, Closed Won the official stages?
- Status: resolved by admin permission to assume
- Resolution (agent inference): treat Proposal, Negotiation, Closed Won (and
  Closed Lost, since the picklist still lists it) as the in-use sales
  stages. Filter reports/automations on those short labels. The picklist is
  effectively non-restricted; the legacy Salesforce defaults
  (Prospecting, Qualification, Needs Analysis, Value Proposition,
  Id. Decision Makers, Perception Analysis, Proposal/Price Quote,
  Negotiation/Review) should not be used by agents. Confidence medium.

## 2026-05-16 - Opportunity Type picklist mismatch

- Question: Are New Business, Expansion, Renewal the official Opp types?
- Status: resolved by admin permission to assume
- Resolution (agent inference): yes. New Business, Expansion, Renewal are
  the in-use values and are the labels the ARR_Field_Type_Match validation
  rule expects. Agents must not use the long Salesforce defaults
  (Existing Customer - Upgrade, etc.). Confidence medium.

## 2026-05-16 - Account.Type and the Churned value

- Question: Should Churned be added to Account.Type so
  Account_Type_Forward_Only can complete?
- Status: resolved by admin permission to assume
- Resolution (agent inference): treat Customer and Prospect as the only
  reachable Account.Type values today. Churned is referenced by the
  validation rule but is not selectable, so the lifecycle effectively stops
  at Customer. Until the admin adds the picklist value, agents must not
  attempt to set Account.Type to Churned. Confidence medium.

## 2026-05-16 - test_field__c validation rule

- Question: Should we deactivate / delete test_field_must_be_gt_100?
- Status: resolved by admin permission to assume
- Resolution (agent inference): treat the rule as stale. The field
  test_field__c does not exist on Account; the rule cannot fire today
  because there is no field to evaluate. Agents must not create or restore
  a test_field__c field on Account without first deactivating this rule.
  Recommend cleanup at the next admin pass. Confidence medium.

## 2026-05-16 - ARR_Legacy__c status

- Question: Is ARR_Legacy__c officially deprecated?
- Status: resolved by admin permission to assume
- Resolution (agent inference): treat ARR_Legacy__c as deprecated. Do not
  read it in pipeline / ARR reports. Do not write it on new or existing
  records. Use ARR_New__c (for New Business) and ARR_Expansion__c (for
  Expansion / Renewal). The one populated record (Summit Software - Upsell)
  is historical and already mirrored in ARR_Expansion__c. Confidence medium.

## 2026-05-16 - Sample custom fields on Account / Opportunity

- Question: Are the Developer Edition Sample fields intentionally unused?
- Status: resolved by admin permission to assume
- Resolution (agent inference): treat all listed Sample fields as unused
  scaffolding. Agents must not surface them in reports or recommend them
  for writes. They are documented as deprecated in data-model.md.
  Confidence medium.

## 2026-05-16 - Case routing and ownership

- Question: Is manual case ownership intentional? Triage rules?
- Status: resolved by admin permission to assume
- Resolution (agent inference): cases are routed manually today. There are
  no queues, no assignment rules, and Type is null on every existing case.
  Critical security cases appear to default to Shubham Goel (System
  Administrator / VP Sales) based on case 00001122. Agents should treat
  Shubham as the owner of last resort for Critical or Security cases.
  Confidence medium.

## 2026-05-16 - Flow / automation gaps

- Question: Any active Flows, Process Builders, Workflow Rules, or Approval
  Processes?
- Status: resolved by admin permission to assume
- Resolution (agent inference): treat the org as having only validation
  rules and packaged Apex. No flows, no triggers, no approval processes
  were observed via read-only Tooling API. Agents should not assume any
  automation side effects beyond the validation rules listed in
  memory/automation.md. Re-confirm if the admin later mentions a flow.
  Confidence medium.

## 2026-05-16 - Reporting and KPI conventions

- Question: Any report folders, dashboards, or KPIs to follow?
- Status: resolved by admin permission to assume
- Resolution (agent inference): there are no org-authored reports or
  dashboards today; all observed reports/dashboards are packaged
  scaffolding (Einstein Bot, Enablement, Flow Orchestration). Agents should
  build ad-hoc SOQL aggregates rather than rely on saved reports. Suggested
  default KPIs derivable from observed data: pipeline by StageName and
  Owner, total ARR by ARR_New__c + ARR_Expansion__c per Account_Tier__c,
  open cases by Priority and Owner. Confidence medium.
