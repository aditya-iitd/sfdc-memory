# Integrations

Integration and external-system context for this Salesforce org.

## Verification Rule

Integration facts below were observed on 2026-05-16. Re-check connected apps,
external client apps, named credentials, remote site settings, auth providers,
installed packages, scheduled jobs, and users before relying on them.

## Installed Packages

| Package | Namespace | Version | Notes |
| --- | --- | --- | --- |
| Developer Edition | `devedapp` | 0.9.0.2 | Provides Developer Edition app/classes |

## Connected Apps And Credentials

- `ConnectedApplication` query returned no rows.
- No `NamedCredential` metadata found.
- No `ExternalDataSource` metadata found.
- No `AuthProvider` metadata found.
- Remote site setting `ApexDevNet` exists, created by OrgFarm EPIC on
  2026-01-09.

## Integration Users

| User | Type/Profile | Notes |
| --- | --- | --- |
| Platform Integration User | CloudIntegrationUser / no profile | Active generated integration user |
| Integration User | Analytics Cloud Integration User | Active |
| Security User | Analytics Cloud Security User | Active |
| Slackbot | AutomatedProcess / no profile | Active user `slackbot@00dg5000003frieeas.ext`; no last login observed |

## Scheduled Integration-Like Jobs

- `Metalytics Data Loader Job for Org : 00Dg5000003FriE` exists as a waiting
  scheduled job.
- Program milestone/status scheduled jobs also exist.
- Inspect `CronTrigger`, related packages, and job details before assuming no
  background writes occur.
