# Salesforce Org Memory

Organizational memory for a Salesforce admin agent.

## Current confidence

Initial onboarding completed 2026-05-16 against the org
Walrus Holdings Pte. ltd. (alias orgfarm-dev, 00Dg5000003FriEEAS).
The admin (Shubham Goel) confirmed that:

- orgfarm-dev must be treated as a production org by all agents.
- The second authenticated org (00DgK00000LMHW8UAP) must be ignored.
- For all other onboarding questions, the agent was authorized to assume
  based on read-only exploration. Those assumptions are documented in
  memory/open-questions.md (status resolved) and memory/policies.md.

Confidence is medium overall and is noted in each section.

This repository is context, not authority. Agents must verify operational
facts against live Salesforce before acting, and must ask the admin again
when a new ambiguous case appears.

## Files

Read these files before planning or answering Salesforce work:

1. memory/policies.md
2. memory/org-overview.md
3. memory/data-model.md
4. memory/security-and-access.md
5. memory/automation.md
6. memory/business-processes.md
7. memory/integrations.md
8. memory/open-questions.md
9. memory/changelog.md

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
- There is intentionally no CLAUDE.md; this repository is memory, not a
  system prompt.
