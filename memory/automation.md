# Automation

Status: initial discovery from read-only inspection on 2026-05-16. Confidence
medium unless noted.

## Validation rules

Seven active validation rules were found via Tooling API (`ValidationRule`).
Treat all as load-bearing until the admin says otherwise.

### Opportunity

- `Closed_Stage_Immutable` - prevents modification of `StageName` on closed
  opportunities. Error: "Cannot modify Stage on closed Opportunities. Closed
  deals are final."
- `ARR_Field_Type_Match` - ensures the correct ARR custom field is used for
  the opportunity Type. Error: "ARR field mismatch: Use ARR_New__c for New
  Business opportunities, ARR_Expansion__c for Expansion / Renewal." This is
  the rule that effectively defines `ARR_Legacy__c` as not the canonical
  source.
- `Min_Opportunity_Amount_By_Tier` - enforces minimum opportunity Amount
  based on Account Tier: Enterprise >= $50,000, Mid-Market >= $10,000.
- `Discount_Max_Sub100k` - `Discount_Percent__c` must be less than 10% when
  Amount is below $100,000.

### Account

- `Strategic_Flag_Permanent` - once `Is_Strategic__c` is true it cannot be
  set back to false.
- `Account_Type_Forward_Only` - `Account.Type` can only progress forward in
  the lifecycle `Prospect -> Customer -> Churned`. Note: `Churned` is NOT in
  the picklist values returned by describe (only `Customer` and `Prospect`
  appear in data); the rule references a value that does not yet exist in
  the picklist metadata. Admin question logged.
- `test_field_must_be_gt_100` - ensures `test_field__c` is greater than 100
  when populated. The `test_field__c` field is NOT in the Account describe
  output. Likely a leftover validation rule referencing a deleted or never-
  created field. High risk if any process tries to set the field. Admin
  question logged.

## Apex

- Only 4 Apex classes exist in the org and all are in the `devedapp`
  namespace shipped by the Developer Edition package
  (`DeveloperEditionUtils`, `DeveloperEditionUtilsTest`,
  `PostInstallScript`, `PostInstallScriptTest`).
- No org-authored Apex classes (`NamespacePrefix = null`).
- No Apex triggers (`SELECT FROM ApexTrigger` returned 0 rows).

## Flows / Process Builder / Workflow Rules

- `Flow` SOQL queryable but `FlowDefinitionView` is not supported on this
  instance through the Tooling API used here. A direct `Flow` describe shows
  no `Name`-style columns; further discovery required.
- No flows were observed via tooling-friendly queries used so far. Confidence
  low - additional discovery (Metadata API listing of `Flow` and
  `FlowDefinition`) should be run in a later cycle.
- Process Builder, Workflow Rules, and Approval Processes: `ProcessDefinition`
  is not a queryable SOQL type in this org. No `ProcessInstance` history was
  inspected yet.

## Scheduled jobs / async

- Not yet enumerated. `CronTrigger`, `AsyncApexJob`, and `BackgroundOperation`
  not queried.

## Operational risks observed

- Validation rule `test_field_must_be_gt_100` references `test_field__c`
  which does not exist on Account. Any code that creates the field, or any
  data load that includes a column with that name, will trip a rule that
  may have been forgotten. Mark as risky.
- Validation rule `Account_Type_Forward_Only` references a `Churned` value
  that is not in the current `Account.Type` picklist. Either the picklist is
  missing the value or the rule is incomplete.
- Several picklists allow values that no longer match in-use values; agents
  must not rely on picklist labels alone for filtering or routing.

## Next discovery targets

- Listing of flow / process metadata via Tooling API (`FlowDefinition`,
  `Flow`).
- `CronTrigger` and `AsyncApexJob` snapshot.
- Approval processes via Metadata-API listing.
