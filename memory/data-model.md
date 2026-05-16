# Data Model

Status: initial discovery from read-only inspection on 2026-05-16. Confidence
medium unless noted.

## Objects in active use

No org-authored custom (`__c`) objects were found via Tooling API
(`CustomObject` returned 0 rows; no entities with namespace `null` matched
`%__c`). The data model is standard objects plus custom fields on Account
and Opportunity.

### Account (KeyPrefix 001)

- Row count: 7. Owners observed: `Sarah Chen` (Sales Manager) and
  `John Smith` (Account Executive). Confidence high.
- Standard `Type` values present in data: `Customer`, `Prospect`. The picklist
  metadata still advertises legacy values such as `Customer - Direct`,
  `Customer - Channel`, `Channel Partner / Reseller`, `Installation Partner`,
  `Technology Partner`, `Other`. These legacy values are NOT in use - see
  Deprecated Fields below.
- Custom fields observed:
  - `Account_Tier__c` (picklist): `Enterprise`, `Mid-Market`, `SMB`.
    Drives the `Min_Opportunity_Amount_By_Tier` validation rule (Enterprise
    >= $50,000, Mid-Market >= $10,000). High confidence in usage.
  - `Is_Strategic__c` (boolean): once set to true cannot be unset (validation
    rule `Strategic_Flag_Permanent`).
  - `Payment_Terms__c` (picklist): `Net 15`, `Net 30`, `Net 45`, `Net 60`,
    `Due Upon Receipt`. Observed values: Net 15 / Net 30 / Net 60.
  - `CustomerPriority__c`, `SLA__c`, `Active__c`, `UpsellOpportunity__c`,
    `NumberofLocations__c`, `SLASerialNumber__c`, `SLAExpirationDate__c` -
    legacy "Sample" custom fields shipped with Developer Edition. None of
    the 7 accounts have data in these fields. Treat as unused; verify with
    admin.

### Opportunity (KeyPrefix 006)

- Row count: 7. Confidence high.
- Standard `StageName` values present in data: `Proposal`, `Negotiation`,
  `Closed Won`. The picklist metadata still advertises Salesforce default
  values (`Prospecting`, `Qualification`, `Needs Analysis`, ...,
  `Proposal/Price Quote`, `Negotiation/Review`, `Closed Won`, `Closed Lost`).
  This is a significant mismatch - see Deprecated Fields and Open Questions.
- Standard `Type` values present in data: `New Business`, `Expansion`,
  `Renewal`. Picklist metadata advertises Salesforce defaults
  (`Existing Customer - Upgrade`, `Existing Customer - Replacement`,
  `Existing Customer - Downgrade`, `New Customer`). The in-use values
  override the defaults via a non-restricted picklist or API writes.
- Custom fields observed:
  - `ARR_New__c` (currency): set on opportunities where
    `Type = 'New Business'`.
  - `ARR_Expansion__c` (currency): set on `Expansion` and `Renewal` opps.
  - `ARR_Legacy__c` (currency): only populated on one Closed Won opp
    (`Summit Software - Upsell`, value 30000). Likely legacy mirror - see
    Deprecated Fields.
  - `Discount_Percent__c` (percent): no records currently populate it; the
    `Discount_Max_Sub100k` validation rule constrains it (<10% when
    Amount < $100,000).
  - `DeliveryInstallationStatus__c`, `TrackingNumber__c`, `OrderNumber__c`,
    `CurrentGenerators__c`, `MainCompetitors__c` - legacy "Sample"
    Opportunity custom fields shipped with Developer Edition. No data
    populated. Treat as unused; verify with admin.

### Case (KeyPrefix 500)

- Row count: 5. Owners include Sarah Chen, John Smith, and Shubham Goel.
  Confidence high.
- `Type` is null on all observed cases. Statuses in use: `New`, `In Progress`,
  `Closed`. Priorities: `Low`, `Medium`, `Critical`.
- No custom fields detected on Case.

### Empty objects

- `Contact`: 0 records.
- `Lead`: 0 records.
- No `RecordType` rows exist for any object (high confidence).

### User

- 13 user rows. See `memory/security-and-access.md`.

## Relationships

- `Opportunity.AccountId -> Account.Id` is the primary commercial link.
- `Case.AccountId` is populated on all 5 cases. `ContactId` is null on every
  case.

## Deprecated Fields

Document these before any agent queries, reports, automations, or writes
touch them.

### Opportunity.ARR_Legacy__c

- Signal: only populated on one historical Closed Won record where
  `ARR_Expansion__c` has the same value. Looks like a backfill of an earlier
  field, now superseded by `ARR_New__c` / `ARR_Expansion__c`.
- Whether records still contain values: yes, on one record (Summit Software -
  Upsell, $30,000).
- Likely replacement: `ARR_New__c` for New Business, `ARR_Expansion__c` for
  Expansion / Renewal. Enforced by validation rule `ARR_Field_Type_Match`.
- Risk if used: agents that read or write `ARR_Legacy__c` will see stale
  data and may double-count ARR in reports.
- Confidence: medium. Open question logged for admin to confirm the field is
  truly legacy and whether new writes should be blocked.

### Account "Sample" custom fields

- Fields: `CustomerPriority__c`, `SLA__c`, `Active__c`,
  `UpsellOpportunity__c`, `NumberofLocations__c`, `SLASerialNumber__c`,
  `SLAExpirationDate__c`.
- Signal: all 7 Account records have null in these fields. Labels and types
  match the Salesforce sample app pattern that ships with Developer Edition.
- Whether records still contain values: no observed values.
- Likely replacement: none used today. May be safe to ignore in reports.
- Risk if used: building reports or agent flows on these fields will return
  empty data and look broken.
- Confidence: medium. Admin question logged.

### Opportunity "Sample" custom fields

- Fields: `DeliveryInstallationStatus__c`, `TrackingNumber__c`,
  `OrderNumber__c`, `CurrentGenerators__c`, `MainCompetitors__c`.
- Signal: all 7 Opportunity records have null in these fields. Same Developer
  Edition sample-app heritage.
- Whether records still contain values: no observed values.
- Likely replacement: none used today.
- Risk if used: empty reports / misleading agent answers.
- Confidence: medium. Admin question logged.

### Opportunity.StageName legacy picklist values

- Signal: the picklist still includes Salesforce-default values
  (`Prospecting`, `Qualification`, `Needs Analysis`, `Value Proposition`,
  `Id. Decision Makers`, `Perception Analysis`, `Proposal/Price Quote`,
  `Negotiation/Review`, `Closed Lost`). None appear on actual records.
  The in-use stages are `Proposal`, `Negotiation`, `Closed Won`.
- Whether records still contain values: no.
- Likely replacement: the short labels actually in use.
- Risk if used: filtering reports / dashboards / agent queries on the legacy
  labels will return zero rows.
- Confidence: medium. Open question: are short labels the official set, and
  is the underlying picklist intentionally non-restricted?

### Account.Type legacy picklist values

- Signal: picklist still includes `Customer - Direct`, `Customer - Channel`,
  `Channel Partner / Reseller`, `Installation Partner`,
  `Technology Partner`, `Other`. None appear on actual records. In-use
  values are just `Customer` and `Prospect`.
- Whether records still contain values: no.
- Likely replacement: short labels `Customer` / `Prospect`.
- Risk if used: filtering or routing on the long labels will fail silently.
- Confidence: medium. Admin question logged.

### Opportunity.Type legacy picklist values

- Signal: picklist still includes `Existing Customer - Upgrade`,
  `Existing Customer - Replacement`, `Existing Customer - Downgrade`,
  `New Customer`. In-use values are `New Business`, `Expansion`, `Renewal`.
- Whether records still contain values: no.
- Likely replacement: the in-use short labels.
- Risk if used: validation rule `ARR_Field_Type_Match` and reporting expect
  the short labels. Long labels will not match.
- Confidence: medium. Admin question logged.
