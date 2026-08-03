# Permissions Required

Every privilege Jade Snowpulse AI requests at install time, and exactly
what it's used for. You'll be prompted to grant each of these through
Snowsight's standard install flow.

| Privilege | Why the app needs it |
|---|---|
| `CREATE COMPUTE POOL` | Runs the app's container service. You can decline this and create a compute pool yourself instead if you'd rather manage sizing manually. |
| `BIND SERVICE ENDPOINT` | Exposes the app's dashboard and API so your users can reach it. |
| `CREATE WAREHOUSE` | Creates a small, dedicated warehouse used only to keep the app's own pre-aggregated data fresh — never used for anything else, and automatically suspends within seconds of each refresh. |
| `EXECUTE TASK` | Runs scheduled background jobs (alert evaluation, usage rollups) independent of whether anyone has the app open. Alerts still work on-demand without this — it only affects scheduled/background evaluation. |
| `EXECUTE MANAGED TASK` | An alternative form of the same privilege above — granting both is safe. |
| `IMPORTED PRIVILEGES ON SNOWFLAKE DB` | Lets one specific scheduled background job read account cost data directly. On-demand alert evaluation from the UI doesn't need this. |
| `SNOWFLAKE.CORTEX_USER` | Calls Snowflake Cortex for AI-generated insights and query optimization, using only models your admin has explicitly allow-listed. |

## Restricted caller's rights

This app also runs under Snowflake's restricted caller's rights model —
a separate mechanism from the privileges above. In short: the app can
only see what the logged-in user's own role already has access to,
unless you separately grant it visibility with `GRANT CALLER`. One such
grant is required for full functionality — see step 2 of the
[Installation Guide](01-installation-guide.md). Skipping it doesn't cause
an error; it causes certain checks to silently report "not found" instead
of real data, so don't skip it.

## What the app does *not* do

- No data is sent to any AI provider other than Snowflake Cortex, which
  runs inside your own account.
- No credentials or API keys are ever requested or stored by this app.
- The app has no login system of its own — access is entirely governed
  by your own Snowflake account's authentication (including SSO, if your
  organization uses it).
