# Integrations

Status: initial discovery from read-only inspection on 2026-05-16. Confidence
medium.

## External systems and connected apps

- `ConnectedApplication` query returned 0 rows. Either there are no
  org-installed connected apps that this user can see, or the user does not
  have visibility into managed connected apps from system providers. Admin
  question logged.
- `NamedCredential` query returned 0 rows. No outbound authenticated
  integrations are defined as Named Credentials.
- Salesforce CLI is currently authenticated as `PlatformCLI`
  (`clientId = PlatformCLI`) - that is the official Salesforce CLI OAuth
  client, not an org-specific connected app.

## Installed packages

- One installed package: `Developer Edition` (`devedapp` namespace, v0.9).
  This is the Salesforce Org Farm scaffolding; it ships four Apex classes
  (`DeveloperEditionUtils`, `DeveloperEditionUtilsTest`,
  `PostInstallScript`, `PostInstallScriptTest`). Not a business integration.

## Integration / API users

- `Integration User`, `Platform Integration User`, `Data.com Clean`,
  `Chatter Expert`, `Security User`, `Automated Process` - the usual
  Salesforce platform service accounts. None of them indicate an org-
  authored integration.
- `Slackbot` (`slackbot@00dg5000003frieeas.ext`) - a custom-looking user
  with no profile, no role, never logged in. The naming pattern suggests an
  intended Slack <-> Salesforce integration user. The admin should confirm
  what runs as this user, whether it is wired up to a connected app, and
  whether any sync direction (Slack -> SFDC vs SFDC -> Slack) is expected.

## Webhooks / outbound messages / platform events

- Not yet inspected. `OutboundMessage`, `PlatformEvent`-style topics, and
  `EventChannel` should be checked in a later cycle.

## Sync ownership

- No automation owns any Account, Opportunity, or Case field today, based
  on the lack of Apex triggers, flows, and named credentials. All records
  appear to be manually edited.

## Caveats

- Without a clear list of connected apps and remote sites, we cannot rule
  out an external system pushing data into the org via OAuth or REST. The
  small record counts (7 / 7 / 5) suggest manual entry, but a real
  integration may simply be quiet right now. Admin confirmation needed.
