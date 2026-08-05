# Release Notes

## 1.3 — Current release

- **Cost & Usage Forecast**: a 30-day projection of compute cost and
  Cortex/AI spend based on recent trend.
- **Data Refresh self-service recovery**: resume a suspended background
  data refresh directly from the app, without contacting support.
- Improved noise filtering on RBAC change detection, so only genuinely
  meaningful role changes surface.
- Fixed an install-time activation issue that could block provisioning of
  the compute pool and service on a subset of accounts.
- Corrected three health checks (Duplicate Data Detection, Stage File
  Cleanup, Time Travel / Fail-safe Storage Costs) that were reading from
  the wrong internal data source and could under-report on some accounts.
- Extended the missing-grant self-diagnosis warning described in the
  [FAQ](04-faq-troubleshooting.md) to all nine warehouse-related health
  checks, not just two.

## 1.2

- **Action Queue**: a persistent, ranked backlog of findings with
  cross-run history, replacing a flat re-snapshotted results view.
- **Alerting**: budget threshold, credit-spike, and health-check-severity
  rules, with optional Slack/Teams delivery.
- **Chargeback Report**: warehouse cost by tag, with an estimated
  per-user cost breakdown.
- **AI Usage Monitoring**: expanded to show total account-wide Cortex
  spend across all sources, not just this app's own usage.
- **Exec Digest**: an automatically generated weekly summary of savings
  tracked and top risks/wins.
- **RBAC change detection** and **cross-domain correlation** of related
  findings into single, combined items.
- **Root-cause detail** added to credit-spike findings — the specific
  contributing warehouse and user, not just the dollar amount.

## 1.1

- Performance overhaul: Dashboard and Health Check pages now load from
  pre-aggregated data instead of running live queries on every visit.

## 1.0 — Initial release

- 45 automated health checks across Compute, Storage, Query, and
  Governance.
- AI-generated insights via Snowflake Cortex.
- Query Optimizer: AI-generated query rewrites with explanations.
- Runs entirely inside your own Snowflake account via Snowpark Container
  Services — no external API keys, no data leaving your account.
