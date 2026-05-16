# Org Overview

Status: initial discovery from read-only inspection on 2026-05-16. Confidence
medium unless noted.

## Identity

- Org name: `Walrus Holdings Pte. ltd.` (high confidence; `Organization.Name`)
- Org Id: `00Dg5000003FriEEAS`
- Edition: Developer Edition (`Organization.OrganizationType`)
- Instance: `IND168`
- My Domain host: `orgfarm-16e9a62082-dev-ed.develop.my.salesforce.com`
- `IsSandbox`: false. Treat as a Developer Edition org used for demo/test;
  business meaning unconfirmed (open question for admin).
- Created: 2026-01-09. Demo/training-style content (Acme Corp, Megacorp,
  TechStart, etc.) suggests this is a non-production demo org. Confidence
  medium pending admin confirmation.
- `NamespacePrefix`: null (no org namespace).
- Default locale and language: `en_US`. Fiscal year starts in January.

## Salesforce CLI aliases

Two authenticated orgs are known to `sf org list`:

| Alias        | Username                                | Org Id              | Notes                                  |
|--------------|-----------------------------------------|---------------------|----------------------------------------|
| `orgfarm-dev`| `shubham.c5391323dee2@agentforce.com`   | `00Dg5000003FriEEAS`| Currently the default `target-org`.    |
| (no alias)   | `shubham.91664b5e4190@agentforce.com`   | `00DgK00000LMHW8UAP`| Second developer org. Purpose unknown. |

All Salesforce work described in this memory targets `orgfarm-dev` unless
otherwise stated. Confidence high (evidence: `sf config get target-org`,
`sf org display`).

## Installed packages

- `Developer Edition` package, namespace `devedapp`, version 0.9. Installed by
  Salesforce Org Farm; contributes 4 Apex classes (`DeveloperEditionUtils`,
  `DeveloperEditionUtilsTest`, `PostInstallScript`, `PostInstallScriptTest`).
  Treat as platform/template scaffolding, not org-authored automation.

## Limits and licensing signals

- Developer Edition limit profile (e.g. `DailyAnalyticsDataflowJobExecutions`
  max 60, `ContentDistViewLimit` max 50,000). Most counters are at max,
  consistent with low usage.
- Many Subscription Management, Commerce, Enablement, Agentforce, and Copilot
  permission sets are pre-provisioned. Whether any of these features are
  actually adopted is unknown - admin question logged.

## Default org caveat

- No `sf config get target-org` value is currently set in this project; commands
  must pass `--target-org orgfarm-dev` explicitly. Confidence high.

## Environment type

- Production-style developer org (not a sandbox). Treat with the same care as
  production, but evidence suggests it is currently used as a demo/training
  workspace. Admin must confirm before any agent is given write access.
