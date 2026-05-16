# History

Completed tasks, durable observations, and useful debugging notes for this org.

## Entries

## 2026-05-16 - Initial Salesforce CLI Memory Inspection

- Type: observation
- Summary: Inspected `orgfarm-dev` with Salesforce CLI and populated memory with
  org identity, users, permissions, object model, validation policies,
  automation, integrations, and data-shape notes.
- Salesforce objects or metadata: Organization, User, Profile, PermissionSet,
  PermissionSetAssignment, UserRole, UserLicense, EntityDefinition,
  DuplicateRule, ValidationRule, CustomField, Account, Opportunity, Case,
  CronTrigger, ApexClass, AppDefinition, InstalledSubscriberPackage metadata.
- Result: Memory now captures org-specific context plus a strong instruction to
  verify every fact against Salesforce before reuse.
- Caution: CLI auth listed multiple connected orgs and no configured default
  target. This memory uses `orgfarm-dev`.

## Format

```text
## YYYY-MM-DD - Short Title

- Type: task | context-update | observation | incident
- Summary:
- Salesforce objects or metadata:
- Result:
```
