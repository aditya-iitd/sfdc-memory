# Salesforce Org Memory

Organizational memory for a Salesforce admin agent.

## Current confidence

Initial onboarding pass completed 2026-05-16 against the org
`Walrus Holdings Pte. ltd.` (alias `orgfarm-dev`,
`00Dg5000003FriEEAS`). Confidence is medium overall and is explicitly
noted in each section. Twelve open questions are awaiting the admin and
are tracked in `memory/open-questions.md`.

This repository is context, not authority. Agents must verify operational
facts against live Salesforce before acting, and must ask the human
Salesforce admin when business meaning or policy intent is unclear.

## Files

Read these files before planning or answering Salesforce work:

1. `memory/policies.md`
2. `memory/org-overview.md`
3. `memory/data-model.md`
4. `memory/security-and-access.md`
5. `memory/automation.md`
6. `memory/business-processes.md`
7. `memory/integrations.md`
8. `memory/open-questions.md`
9. `memory/changelog.md`

## Rules

- Store durable organizational context, not raw dumps.
- Do not store secrets, tokens, passwords, private keys, or full sensitive
  values.
- Mark confidence as high, medium, or low.
- Include short evidence notes such as command names, object names, dates,
  or admin confirmations.
- Document deprecated, legacy, unused, and do-not-use fields before
  recommending fields in queries, reports, automations, or writes.
- Prefer concise synthesis over large copied Salesforce output.
- Keep open questions explicit until a human admin resolves them.
- There is intentionally no `CLAUDE.md`; this repository is memory, not a
  system prompt.
