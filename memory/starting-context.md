# Appendix B: Starting Context

This file is seed context supplied by the human admin. It is useful onboarding
input, not final authority. Salesforce metadata, data, validation rules,
permissions, limits, and policies may have changed. Agents must verify current
state with read-only Salesforce queries before relying on this context,
especially before answering sensitive questions or planning writes.

Last supplied: 2026-05-17
Target environment: Salesforce sandbox / `orgfarm-dev`
Source: human-provided starting context
Confidence: medium until verified against live Salesforce

## Salesforce Sandbox Schema

### Account

| Field API Name | Label | Type | Values / Notes |
|---|---|---|---|
| `Id` | Account ID | ID |  |
| `Name` | Account Name | Text |  |
| `Type` | Type | Picklist | Prospect, Customer, Churned |
| `Account_Tier__c` | Tier | Picklist | Enterprise, Mid-Market, SMB |
| `OwnerId` | Owner | Lookup(User) | Assigned rep |
| `Is_Strategic__c` | Strategic | Checkbox | Flags key accounts |
| `Payment_Terms__c` | Payment Terms | Picklist | Net 15, Net 30, Net 45, Net 60, Due Upon Receipt |

### Opportunity

| Field API Name | Label | Type | Values / Notes |
|---|---|---|---|
| `Id` | Opportunity ID | ID |  |
| `Name` | Name | Text |  |
| `AccountId` | Account | Lookup(Account) |  |
| `Amount` | Amount | Currency | Deal value |
| `StageName` | Stage | Picklist | Prospecting, Proposal, Negotiation, Closed Won, Closed Lost |
| `Type` | Type | Picklist | New Business, Expansion, Renewal |
| `OwnerId` | Owner | Lookup(User) | Assigned AE |
| `ForecastCategory` | Forecast | Picklist | Pipeline, Best Case, Commit, Closed |
| `Discount_Percent__c` | Discount % | Percent |  |
| `ARR_New__c` | New ARR | Currency | For new business |
| `ARR_Expansion__c` | Expansion ARR | Currency | For expansions |
| `ARR_Legacy__c` | Legacy ARR | Currency | DEPRECATED |

### Case

| Field API Name | Label | Type | Values / Notes |
|---|---|---|---|
| `Id` | Case ID | ID |  |
| `Subject` | Subject | Text |  |
| `Status` | Status | Picklist | New, In Progress, Escalated, Closed |
| `Priority` | Priority | Picklist | Low, Medium, High, Critical |
| `AccountId` | Account | Lookup(Account) |  |
| `OwnerId` | Owner | Lookup(User) | Assigned agent or queue |

### User

| Field API Name | Label | Type | Values / Notes |
|---|---|---|---|
| `Id` | User ID | ID |  |
| `Name` | Name | Text | Full name |
| `Email` | Email | Email |  |
| `IsActive` | Active | Checkbox | Can log in? |
| `ProfileId` | Profile | Lookup |  |
| `ManagerId` | Manager | Lookup(User) |  |
| `Title` | Title | Text | Job title |

## Profiles, Roles, And Permissions

### Profiles

All non-admin users use the Standard User profile. The bot should enforce
business rules such as ownership checks and discount approvals based on role
hierarchy and context rather than relying only on Salesforce profile-level
permissions.

### Role Hierarchy

```text
VP Sales
`-- Sales Manager
    `-- Account Executive (multiple)

Support Manager
`-- Support Agent (multiple)
```

### Permission Sets

| Permission Set | Description |
|---|---|
| `Forecast_Editor` | Grants ability to edit the `ForecastCategory` field on Opportunities |
| `API_Access` | Grants ability to access Salesforce via API for integrations |

## Additional Context

### User Deactivation Steps

1. Transfer owned Accounts to manager.
2. Transfer owned Opportunities to manager.
3. Transfer open Cases to manager.
4. Set `IsActive = false`.

## Discovery Expectations

The bot should discover additional business rules by exploring the Salesforce
environment rather than having them hardcoded. The sandbox encodes operational
knowledge at multiple levels.

Examples of discovery targets:

1. Field descriptions can contain operating rules. For example, a field
   description might say: "Only use for active contracts - legacy records may
   contain historical values." The bot should read these descriptions and
   respect them when operating on records.
2. Existing validation rules may reject invalid operations. For example, a rule
   might prevent an Opportunity discount from exceeding 30%. The bot should
   discover this proactively before proposing writes.
3. Repeated data patterns may reveal conventions. For example, every Case may
   follow the same naming convention. The bot should notice and document that
   pattern.
4. Partial hints may indicate missing policy. For example, two field
   descriptions may both hint at a special exception for certain accounts, while
   the actual policy is not written anywhere. The bot should recognize the gap,
   ask the context owner/admin a specific question, and document the answer.
