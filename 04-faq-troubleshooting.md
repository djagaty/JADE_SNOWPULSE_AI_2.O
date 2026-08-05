# FAQ / Troubleshooting

## A warehouse-related health check warns that the app "can't see" my warehouses

This is expected, self-diagnosing behavior, not a bug — you're missing a
required one-time setup grant. The warning itself names the exact
statement to run; as `ACCOUNTADMIN`:
```sql
GRANT INHERITED CALLER USAGE, MONITOR ON ALL WAREHOUSES IN ACCOUNT TO APPLICATION <app_name>;
```
See step 2 of the [Installation Guide](01-installation-guide.md). This is
the single most common setup gap — the app's role-scoped session can't see
your warehouses at all until this grant exists. Every warehouse-related
check (Idle Warehouses, Oversized Warehouses, Auto-suspend Settings,
Warehouse Consolidation, Multi-cluster Efficiency, Auto-resume Settings,
Warehouse Utilization, Resource Monitor Coverage, and Cost Allocation
Accuracy) detects this gap and returns the warning above rather than a
silent "no warehouses found" pass — re-run Health Checks after granting.

## AI-generated insights or Query Optimizer aren't working

Make sure at least one Cortex model is allow-listed:
```sql
SELECT cortex_model, is_allowed, is_default FROM CORE.LLM_CONFIG WHERE is_allowed = TRUE;
```
If this returns no rows, add one from the **Admin → LLM Configuration**
page or via `INSERT INTO CORE.LLM_CONFIG`. The app never picks a model on
its own.

## A Dynamic Table shows "Suspended" on the Data Refresh page

Click **Resume** next to that item — this is a self-service recovery
action built into the app. If it recurs repeatedly, check whether the
warehouse behind it is being blocked by one of your account's resource
monitors reaching its quota; raising the quota (or waiting for the
monitor's reset) typically resolves it.

## The Dashboard's "Last updated" timestamp doesn't move after clicking Refresh

This is expected, not a bug — the timestamp reflects the underlying
data's actual last refresh, not the moment the page loaded. If the
underlying refresh schedule shows as suspended on the Data Refresh page,
resolve that first (see above) and the timestamp will update on the next
scheduled refresh.

## AI Assistant / chat isn't in the navigation

Expected — this feature is intentionally disabled in the current release.

## Alerts aren't being delivered to Slack/Teams

1. Confirm the alert rule's **Notification Channel** is set to "Webhook"
   and a valid incoming webhook URL is set.
2. Confirm you authorized the outbound network connection when prompted
   (required the first time you configure a webhook).
3. Microsoft Teams specifically requires the newer **Workflows**
   app-based webhook — the older "Incoming Webhook" connector type has
   been retired by Microsoft. If your Teams webhook URL was created
   before mid-2026, it may need to be recreated using Workflows.
4. Delivery failures are recorded per-alert and visible in-app — check
   there first before assuming the rule itself isn't firing.

## Does this app send my data anywhere outside my Snowflake account?

No. The only AI backend used is Snowflake Cortex, which runs inside your
own account. No external API keys are used, and no application data
leaves your account as part of normal operation.

## A health-check result says "heuristic" or mentions a sample/proxy signal — is that reliable?

A small number of checks are explicitly disclosed as using a best-effort
signal rather than an exact measurement (for example, checks based on
text-pattern matching, or checks bounded to a representative sample
rather than every object in the account). This is stated directly in
that check's own result message. Treat these as a strong signal worth
investigating, not a definitive answer — the same way you'd treat any
automated heuristic.

## Still stuck?

See [Support](README.md#support) for contact details.
