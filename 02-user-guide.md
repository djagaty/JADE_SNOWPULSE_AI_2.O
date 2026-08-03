# User Guide

A walkthrough of each page in Jade Snowpulse AI.

## Dashboard

Cost, usage, and storage charts for your account, backed by
pre-aggregated data so pages load in seconds instead of paying live query
cost on every visit. Each page shows its own "Last updated" timestamp,
reflecting the underlying data's actual last refresh — not the moment you
loaded the page.

## Health Checks

45 automated checks across four categories — Compute, Storage, Query, and
Governance — each scored with a severity and an estimated dollar savings
where applicable. Every result includes an AI-generated insight (via
Snowflake Cortex) explaining what the finding means and what to do about
it. A handful of checks are explicitly disclosed as heuristic or
sample-bounded rather than exact measurements — this is called out
directly in that check's own result message, not hidden.

## Action Queue

A persistent, ranked backlog of every open finding — not a flat,
re-snapshotted list you have to re-triage every time you look. Each item
tracks how long it's been open, how many times it's recurred, and a
running total of potential savings. Give any item a status (Open →
Snoozed → Resolved → Won't Fix) and an assignee. Certain governance
findings include a "Fix it" button that generates copy-ready remediation
SQL for you to review and run yourself.

Beyond individual health-check findings, the queue also surfaces two
additional signal types automatically: **RBAC changes** (a daily snapshot
flags when a role's grants change unexpectedly — with routine,
Snowflake-managed housekeeping roles filtered out so only genuinely
meaningful changes show up) and **correlated risk** (when the same
warehouse shows up in more than one separate finding — e.g. no resource
monitor *and* significant untagged spend — it's combined into a single
item instead of appearing as several disconnected ones).

## Query Optimizer

Paste a SQL query and get a Cortex-generated rewrite, an explanation of
the changes, and an estimated performance improvement.

## AI Usage Monitoring

A dedicated breakdown of Cortex/AI spend — this app's own usage, plus a
rollup of total account-wide Cortex spend across every source (this app,
Snowsight Copilot, Cortex Agents, and similar), so you have one place to
see the full picture instead of piecing it together across separate
account views.

## Alerts

Define rules for budget thresholds, unusual credit spikes, or health-check
severity. Rules are evaluated automatically on a schedule and after every
health-check run. Alerts are always visible in-app, and can optionally be
delivered to a Slack or Microsoft Teams channel.

## Chargeback Report

Warehouse cost broken down by whichever object tag your organization
already applies (e.g. `cost_center`, `team`), with untagged spend called
out separately as "Unallocated" rather than silently dropped. Includes an
estimated per-user cost breakdown for deeper attribution, and a CSV
export.

## Forecast

A 30-day projection of both account-wide compute cost and Cortex/AI
spend, based on your recent usage trend — a clearly labeled estimate, not
a guarantee, with plain-language insights about the overall direction.

## Exec Digest

A weekly, automatically generated summary — total savings tracked,
what's been resolved, and the top risks and wins — viewable in-app and
optionally pushed to Slack or Teams.

## Data Refresh

See and adjust how often the underlying data behind Dashboard, Health
Checks, and AI Usage Monitoring refreshes. If any item shows as
suspended, a "Resume" button is available to bring it back without
needing to contact support.

## Query Optimizer / AI-generated content

All AI-generated content in this app — health-check insights and query
rewrites — comes from Snowflake Cortex, using whichever model your admin
has allow-listed. No external AI provider is ever used, and no data
leaves your Snowflake account as part of generating this content.
