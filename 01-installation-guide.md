# Installation Guide

Steps to take after installing Jade Snowpulse AI from the Snowflake
Marketplace into your account.

## 1. Grant access to your admin role

The app defines a single application role, `app_admin`. Map it onto
whichever of your own account roles should have access:

```sql
GRANT APPLICATION ROLE <app_name>.app_admin TO ROLE <your_admin_role>;
```

Replace `<app_name>` with whatever name you gave the application at
install time (visible in Snowsight under Data Products → Apps).

## 2. Required one-time grant — let the app see your warehouses

Run this once, as `ACCOUNTADMIN`:

```sql
GRANT INHERITED CALLER USAGE, MONITOR ON ALL WAREHOUSES IN ACCOUNT TO APPLICATION <app_name>;
```

**This step is required, not optional.** Without it, the app's
identity-scoped session can't see your warehouses at all, which affects
every warehouse-related health check (Idle Warehouses, Oversized
Warehouses, Auto-suspend Settings, Warehouse Consolidation, Multi-cluster
Efficiency, Auto-resume Settings, Warehouse Utilization, Resource Monitor
Coverage, and Cost Allocation Accuracy). Each of these checks detects the
gap and returns a warning naming the exact `GRANT INHERITED CALLER`
statement above — it does not silently report a false "pass" or "no
warehouses found," so if you see that warning, this is the fix.

## 3. Allow-list a Cortex model

From the app's **Admin → LLM Configuration** page, or directly:

```sql
INSERT INTO CORE.LLM_CONFIG (cortex_model, is_allowed, is_default)
VALUES ('llama3.1-70b', TRUE, TRUE);
```

Pick any Cortex model your account has access to. This is required before
AI-generated insights or the Query Optimizer will work — the app never
selects a model automatically.

## 4. (Optional) Create an alert rule

From the app's **Alerts** page, or directly:

```sql
INSERT INTO CORE.ALERT_RULES (rule_name, rule_type, threshold_value, notification_channel)
VALUES ('Monthly budget alert', 'BUDGET_THRESHOLD', 5000, 'IN_APP');
```

Rules evaluate automatically on an hourly schedule, plus after every
health-check run. To also deliver alerts to Slack or Microsoft Teams, set
**Notification Channel** to "Webhook" on the Alerts page and paste your
incoming webhook URL — you'll be prompted to authorize the outbound
network connection the first time you do this.

## 5. (Optional) Tag your warehouses for the Chargeback Report

The Chargeback Report groups cost by whichever Snowflake object tag your
organization already applies:

```sql
ALTER WAREHOUSE my_warehouse SET TAG cost_center = 'finance';
```

No additional app-side setup is needed — the report reads the tag
directly.

## 6. (Optional) Set an App URL for alert/digest deep links

On the **Alerts** page, set the **App URL** field to your app's public
endpoint so Teams alert cards and the weekly Exec Digest can include a
working "Open Action Queue" link.

## Next steps

See the [User Guide](02-user-guide.md) for a walkthrough of each page.
