# Learned Context

General org context that does not fit cleanly into policies, permissions,
objects, or automation.

## Verification Rule

Context below was inferred from Salesforce CLI inspection on 2026-05-16. Verify
current Salesforce state before using it.

## Current Context

- This appears to be a Developer Edition / hiring-test org with seeded CRM data,
  not a sandbox (`IsSandbox = false`).
- The business scenario in data is sales and support for customer/prospect
  accounts. Active named business users are Shubham Goel, Sarah Chen, and John
  Smith.
- Several policy-like validation rules look intentionally designed for an agent
  admin exercise: account lifecycle, strategic account permanence, opportunity
  ARR field usage, minimum opportunity amount by account tier, and discount
  limits.
- There are many signs of metadata test activity in Setup Audit Trail:
  temporary Account fields and validation rules were created, changed, and
  deleted in April 2026.
- Treat field names beginning with `sfdc_agent_test_`, `AgtTest`, and obvious
  `test_*` metadata as likely test artifacts unless live inspection and user
  context say otherwise.
- Current records contain inactive-looking picklist values. Memory must not be
  used as authority for picklist choices.

## Format

```text
## YYYY-MM-DD - Short Title

- Source:
- Context:
- Why it matters:
```
