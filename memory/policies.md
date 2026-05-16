# Policies

Business and admin policies for this Salesforce org.

## Verification Rule

All policies below were observed from Salesforce metadata or data on 2026-05-16.
Before enforcing, explaining, or changing them, verify the current validation
rules, fields, and picklist values in Salesforce.

## Current Policies

### Account Lifecycle

- Source: retrieved validation rule `Account.Account_Type_Forward_Only`
- Policy: Account `Type` can only move forward through lifecycle:
  `Prospect -> Customer -> Churned`.
- Formula blocks changing from `Customer` back to `Prospect`, and from `Churned`
  back to `Prospect` or `Customer`.
- Warning: live Account records use `Customer` and `Prospect`, but current
  describe output for Account `Type` did not list `Customer` or `Churned` as
  active picklist values. Verify before changing Account Type.
- Agent behavior: do not downgrade Account Type without explicit confirmation
  and live validation-rule review.
- Confidence: high for rule existence, medium for current lifecycle values.

### Strategic Accounts

- Source: retrieved validation rule `Account.Strategic_Flag_Permanent`
- Policy: once `Account.Is_Strategic__c` is true, it cannot be changed back to
  false.
- Agent behavior: never remove the strategic flag as a routine cleanup. Treat
  any request to unset it as an exception requiring direct verification.
- Confidence: high.

### Opportunity Closed Stages

- Source: retrieved validation rule `Opportunity.Closed_Stage_Immutable`
- Policy: once an Opportunity was previously `Closed Won` or `Closed Lost`, its
  `StageName` cannot be changed.
- Agent behavior: do not propose reopening or restaging closed Opportunities
  without explaining the validation rule and checking for a confirmed exception
  path.
- Confidence: high.

### Opportunity Minimum Amount By Account Tier

- Source: retrieved validation rule `Opportunity.Min_Opportunity_Amount_By_Tier`
- Policy: Enterprise account Opportunities must be at least 50000; Mid-Market
  account Opportunities must be at least 10000.
- Agent behavior: before creating or lowering an Opportunity amount, query the
  parent Account tier and validate the minimum.
- Confidence: high.

### Opportunity ARR Field Usage

- Source: retrieved validation rule `Opportunity.ARR_Field_Type_Match`
- Policy: `New Business` Opportunities should use `ARR_New__c`; `Expansion` and
  `Renewal` Opportunities should use `ARR_Expansion__c`.
- Existing data follows this pattern for the seven observed Opportunities.
- Warning: current describe output for Opportunity `Type` did not list
  `New Business`, `Expansion`, or `Renewal` as active picklist values, even
  though records use those values and the validation rule references them.
- Agent behavior: verify Opportunity Type picklist values and existing data
  before creating or editing Opportunities.
- Confidence: high for rule existence, medium for picklist state.

### Opportunity Discount Limit

- Source: retrieved validation rule `Opportunity.Discount_Max_Sub100k` and
  custom field metadata `Opportunity.Discount__c`.
- Policy: for Opportunities with `Amount < 100000`, `Discount__c` must be less
  than 10.
- Warning: `Discount__c` retrieved through Metadata API but was not visible in
  sObject describe and `SELECT Discount__c FROM Opportunity` failed. No
  FieldPermissions rows were found for it. There is also a visible field
  `Discount_Percent__c`.
- Agent behavior: treat discount fields as inconsistent until verified. Do not
  write discount values without checking describe, FLS, validation metadata, and
  actual queryability.
- Confidence: high for metadata existence, low for live usability.

### Account Test Field Rule

- Source: retrieved validation rule `Account.test_field_must_be_gt_100`
- Policy: if `test_field__c` is populated, it must be greater than 100.
- Warning: this looks like a test artifact. `Account.test_field__c` appears in
  metadata list but not in the standard Account describe field list observed by
  the CLI.
- Agent behavior: avoid relying on or modifying `test_field__c` unless the user
  asks specifically and live metadata confirms it is still present and usable.
- Confidence: medium.

### Duplicate Prevention

- Source: SOQL query on `DuplicateRule`
- Active duplicate rules: Standard Account Duplicate Rule, Standard Contact
  Duplicate Rule, Standard Lead Duplicate Rule.
- Agent behavior: when creating Accounts, Contacts, or Leads, expect standard
  duplicate checks and handle duplicate errors gracefully.
- Confidence: high.

## Data And Security Signals

- Case data includes two critical security-themed cases:
  `Security review - API token rotation needed` and
  `Security incident - unauthorized access attempt`.
- Agent behavior: treat security-related cases and API/token topics as sensitive;
  do not expose secrets, and prefer summaries over copying sensitive details.
- Confidence: medium; verify current case status before referencing.

## Change Management Signals

- Setup Audit Trail shows repeated test metadata creation/removal on Account
  during April 2026, including fields named like `sfdc_agent_test_*`,
  `AgtTest*`, and temporary validation rules.
- Agent behavior: treat these as likely test artifacts unless a current business
  owner confirms otherwise.
- Confidence: medium.
