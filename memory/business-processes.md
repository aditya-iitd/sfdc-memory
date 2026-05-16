# Business Processes

Status: initial inferences from read-only data inspection on 2026-05-16.
Confidence is low for all business-meaning statements until the admin
confirms.

## Sales lifecycle (inferred)

- The org tracks B2B opportunities tied to Accounts. Sales is staffed by:
  - Sarah Chen, Sales Manager (5 accounts, 4 opportunities)
  - John Smith, Account Executive (2 accounts, 3 opportunities)
  - Shubham Goel, VP Sales / System Administrator (overall ownership)
- Opportunity stages in active use: `Proposal`, `Negotiation`, `Closed Won`.
  No `Closed Lost` opps exist yet.
- Opportunity types in active use: `New Business`, `Expansion`, `Renewal`.
- Account types in active use: `Prospect`, `Customer`. A validation rule
  references a forward-only lifecycle `Prospect -> Customer -> Churned`, so
  `Churned` is an expected future state even though the picklist metadata
  does not yet list it.

## ARR convention

- `ARR_New__c` is used for New Business opportunities.
- `ARR_Expansion__c` is used for Expansion and Renewal opportunities.
- `ARR_Legacy__c` is treated as legacy / not to be written; only one closed
  historical record carries a value. See `data-model.md` Deprecated Fields.
- The validation rule `ARR_Field_Type_Match` enforces the split, so agents
  must populate the matching ARR field when creating or updating
  opportunities.

## Discounting

- `Discount_Percent__c` is constrained by `Discount_Max_Sub100k`: must be
  under 10% when Amount is below $100,000. No record currently has a
  populated discount.

## Account tiering

- `Account_Tier__c` (Enterprise / Mid-Market / SMB) drives minimum
  opportunity amounts via `Min_Opportunity_Amount_By_Tier`. Enterprise opps
  must be at least $50,000; Mid-Market opps at least $10,000. SMB has no
  explicit floor in the rule.
- `Is_Strategic__c` is a one-way flag (cannot be unset once true). Currently
  only Megacorp is marked strategic.

## Closed-deal immutability

- Closing an Opportunity (`IsClosed = true`) freezes its Stage via
  `Closed_Stage_Immutable`. Reopening a deal is not currently supported by
  any rule observed; admin question logged about whether reopening is ever
  allowed.

## Support / cases

- Five cases exist with `Type` null on all of them. Statuses observed:
  `New`, `In Progress`, `Closed`. Priorities range Low / Medium / Critical.
- One Critical case (`Security incident - unauthorized access attempt`) is
  owned by Shubham Goel directly; suggests Shubham is the security or
  incident owner of last resort. Admin question logged.
- No queues, no apparent assignment rules. Cases are routed manually.

## Handoffs

- All Cases are linked to Accounts but never to Contacts. Indicates the
  Contact object is not in use yet, so customer identity within a case is
  carried at the Account level only.

## Reporting conventions / KPIs

- Not yet documented. Admin question logged to confirm whether the org has
  any standard report folders, dashboards, or KPIs it expects agents to
  respect.


## Reports / dashboards landscape

- 30 reports exist; all are packaged scaffolding (Einstein Bot, Enablement
  Dashboard, Flow Orchestration). No org-authored reports.
- 2 dashboards exist; both are packaged `Enablement Dashboard` copies.
- There is no business KPI reporting surface today. Admin question logged in
  `memory/open-questions.md`.
