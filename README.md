# Salesforce Org Memory

Simple organizational memory for a Salesforce admin agent.

This repo stores org-specific context for one Salesforce instance: policies,
permission conventions, business rules, object notes, automation notes, and
useful history.

This repo is memory, not a system prompt. Treat these files as context that must
be verified against live Salesforce before use.

## Files

- `memory/instance.md`: org identity and connection notes
- `memory/admin-instructions.md`: instructions explicitly given by human Salesforce admins
- `memory/policies.md`: business and admin policies
- `memory/permissions.md`: how access should be handled in this org
- `memory/objects.md`: important objects, fields, record types, and relationships
- `memory/automation.md`: flows, triggers, jobs, and side effects
- `memory/integrations.md`: connected apps, named credentials, remote sites, packages
- `memory/context.md`: other learned org context
- `memory/history.md`: completed tasks and durable observations

## Read Order

When using this repo as agent memory, read:

1. `memory/instance.md`
2. `memory/admin-instructions.md`
3. `memory/policies.md`
4. `memory/permissions.md`
5. `memory/objects.md`
6. `memory/automation.md`
7. `memory/integrations.md`
8. `memory/context.md`
9. `memory/history.md`

## Rules

- Keep it simple and factual.
- Store org context, not secrets.
- Mark unknowns clearly.
- Update the relevant file when the agent learns something durable.
- Put instructions explicitly given by a human Salesforce admin in
  `memory/admin-instructions.md`.
- Verify every memory fact against Salesforce before relying on it. This repo is
  memory, not authority.
- There is intentionally no `CLAUDE.md`; this repo is organizational memory.
