# Data Model

Status: blank.

Record object, field, relationship, record type, and data-quality context.

Include:

- important standard and custom objects
- key fields, lifecycle/status fields, ownership fields, and lookup/master-detail relationships
- record types, business statuses, and allowed transitions
- data-quality caveats, empty fields, duplicate patterns, and required fields
- fields controlled by automation or integrations
- confidence and evidence for each finding

## Deprecated Fields

Record deprecated, legacy, unused, and do-not-use fields here before any agent
uses fields in queries, reports, automations, or writes.

For each field, include:

- object and field API name
- observed deprecation signal
- whether records still contain values
- known or likely replacement field
- risk if the field is used
- confidence and any admin question needed
