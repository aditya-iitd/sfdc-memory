# Objects

Important Salesforce objects, fields, record types, and relationships.

## Verification Rule

All object and data facts below were observed on 2026-05-16. Before answering,
creating, updating, or deleting records or metadata, re-query Salesforce. This
org has signs of stale metadata and inactive picklist values in live records.

## Important Objects

No custom objects were returned by `sf sobject list --sobject custom`. Current
business data is concentrated in standard CRM objects.

| Object | Purpose | Notes |
| --- | --- | --- |
| Account | Customer/prospect companies | 7 records observed; types include `Customer` and `Prospect`; custom fields include tier, strategic flag, payment terms, SLA fields |
| Opportunity | Deals and ARR tracking | 7 records observed; types include `New Business`, `Expansion`, `Renewal`; stages include inactive-looking values `Proposal` and `Negotiation` |
| Case | Support/security work | 5 records observed; includes critical security cases |
| Contact | People | 0 records observed |
| Lead | Leads | 0 records observed |

## Important Fields

### Account Custom Fields

| Field | Type | Values / Notes |
| --- | --- | --- |
| `Account_Tier__c` | picklist | `Enterprise`, `Mid-Market`, `SMB`; used by Opportunity minimum amount validation |
| `Is_Strategic__c` | checkbox | Once true, validation prevents setting false |
| `Payment_Terms__c` | picklist | `Net 15`, `Net 30`, `Net 45`, `Net 60`, `Due Upon Receipt` |
| `CustomerPriority__c` | picklist | `High`, `Medium`, `Low`; observed records were blank |
| `SLA__c` | picklist | `Gold`, `Silver`, `Platinum`, `Bronze`; observed records were blank |
| `Active__c` | picklist | `Yes`, `No`; observed records were blank |
| `UpsellOpportunity__c` | picklist | `Yes`, `No`, `Maybe`; observed records were blank |
| `NumberofLocations__c` | number | Standard Developer Edition-style custom field |
| `SLASerialNumber__c` | text | Standard Developer Edition-style custom field |
| `SLAExpirationDate__c` | date | Standard Developer Edition-style custom field |

### Opportunity Custom Fields

| Field | Type | Values / Notes |
| --- | --- | --- |
| `ARR_New__c` | currency | Use for `New Business` opportunities |
| `ARR_Expansion__c` | currency | Use for `Expansion` and `Renewal` opportunities |
| `ARR_Legacy__c` | currency | Present; one observed closed-won expansion used legacy ARR |
| `Discount_Percent__c` | percent | Visible/queryable discount percent field |
| `Discount__c` | percent | Retrieved by Metadata API and used by validation, but not visible/queryable in observed describe/SOQL; verify before use |
| `DeliveryInstallationStatus__c` | picklist | `In progress`, `Yet to begin`, `Completed` |
| `TrackingNumber__c` | text | Standard Developer Edition-style field |
| `OrderNumber__c` | text | Standard Developer Edition-style field |
| `CurrentGenerators__c` | text | Standard Developer Edition-style field |
| `MainCompetitors__c` | text | Standard Developer Edition-style field |

### Case Custom Fields

| Field | Type | Values / Notes |
| --- | --- | --- |
| `Product__c` | picklist | `GC1020`, `GC1040`, `GC1060`, `GC3020`, `GC3040`, `GC3060`, `GC5020`, `GC5040`, `GC5060` |
| `PotentialLiability__c` | picklist | `Yes`, `No` |
| `SLAViolation__c` | picklist | `Yes`, `No` |
| `EngineeringReqNumber__c` | text | Engineering request number |

### Contact And Lead Custom Fields

| Object | Field | Type / Values |
| --- | --- | --- |
| Contact | `Level__c` | picklist: `Primary`, `Secondary`, `Tertiary` |
| Lead | `ProductInterest__c` | picklist: `GC1000 series`, `GC3000 series`, `GC5000 series` |
| Lead | `Primary__c` | picklist: `Yes`, `No` |

## Observed Data Shape

### Accounts

| Name | Type | Tier | Strategic | Payment Terms | Owner |
| --- | --- | --- | --- | --- | --- |
| Acme Corp | Customer | SMB | false | Net 30 | John Smith |
| Globex Industries | Customer | Enterprise | false | Net 30 | Sarah Chen |
| Megacorp | Customer | Enterprise | true | Net 60 | Sarah Chen |
| Pinnacle Corp | Prospect | Enterprise | false | Net 30 | Sarah Chen |
| Summit Software | Customer | SMB | false | Net 15 | Sarah Chen |
| TechGlobal Solutions | Customer | Enterprise | false | Net 30 | Sarah Chen |
| TechStart Inc | Prospect | Mid-Market | false | Net 30 | John Smith |

### Opportunities

| Name | Type | Stage | Amount | Account | ARR Pattern |
| --- | --- | --- | --- | --- | --- |
| Acme Corp - Add-on | Expansion | Negotiation | 60000 | Acme Corp | `ARR_Expansion__c = 60000` |
| Acme Corp - Renewal 2026 | Renewal | Negotiation | 25000 | Acme Corp | `ARR_Expansion__c = 25000` |
| Globex Industries - New Business | New Business | Negotiation | 75000 | Globex Industries | `ARR_New__c = 75000` |
| Megacorp - Platform Expansion | Expansion | Proposal | 150000 | Megacorp | `ARR_Expansion__c = 150000` |
| Pinnacle Corp - New Deal | New Business | Closed Won | 200000 | Pinnacle Corp | `ARR_New__c = 200000` |
| Summit Software - Upsell | Expansion | Closed Won | 30000 | Summit Software | `ARR_Expansion__c = 30000`, `ARR_Legacy__c = 30000` |
| TechStart Inc - Expansion | Expansion | Proposal | 45000 | TechStart Inc | `ARR_Expansion__c = 45000` |

### Cases

| Case | Subject | Status | Priority | Owner |
| --- | --- | --- | --- | --- |
| 00001118 | Login issue | In Progress | Medium | John Smith |
| 00001119 | Report not loading | New | Low | Sarah Chen |
| 00001120 | Feature request | Closed | Low | Sarah Chen |
| 00001121 | Security review - API token rotation needed | In Progress | Critical | Shubham Goel |
| 00001122 | Security incident - unauthorized access attempt | Closed | Critical | Shubham Goel |

## Record Types

No active record types were returned by `RecordType WHERE IsActive = true`.

## Picklist Inconsistencies To Verify

- Account `Type` describe output listed standard active values like
  `Prospect`, `Customer - Direct`, and `Customer - Channel`, but records and
  validation rules use `Customer`; validation also references `Churned`.
- Opportunity `Type` describe output listed standard active values like
  `New Customer` and `Existing Customer - Upgrade`, but records and validation
  rules use `New Business`, `Expansion`, and `Renewal`.
- Opportunity active stages include `Proposal/Price Quote` and
  `Negotiation/Review`, while current records use inactive-looking values
  `Proposal` and `Negotiation`.
- Agent behavior: always query current picklist values and existing data before
  creating or updating picklist fields.
