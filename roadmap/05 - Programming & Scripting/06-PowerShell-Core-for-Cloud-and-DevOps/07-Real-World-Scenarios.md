# 07 - Real-World Scenarios & Outage Post-Mortems

## Scenario 1: The Cascading CI/CD Pipeline Disaster (Non-Terminating Errors)

### Context & Incident
A company automated zero-downtime blue/green deployments using a PowerShell Core script running inside a GitHub Actions container. The script wiped the staging database cache, created database schema migrations, and swapped the load balancer route to the new application pods.

### Root Cause
A line in the script failed:
```powershell
Invoke-SqlDatabaseMigration -ConnectionString $prodDbConn
```
Because `$ErrorActionPreference` was not set to `'Stop'`, the command failed silently with a non-terminating error (network timeout to the primary SQL replica). The script **did not halt**; it immediately proceeded to execute:
```powershell
Set-AzApplicationGatewayHttpListener -RoutingRule 'Production' -BackendPool 'GreenPool'
```
The load balancer shifted 100% of user traffic to the Green application fleet, which crashed on boot due to missing database columns, causing a 42-minute high-severity production outage.

### Prevention & Remediation
1. Enforce `$ErrorActionPreference = 'Stop'` at the top of every automation script.
2. Use strict exit codes on failures (`exit 1`) to ensure CI/CD runners mark the step as failed.
3. Validate state transitions with post-step health probes before traffic switching.

---

## Scenario 2: Memory Exhaustion in Large-Scale Object Pipelines

### Context & Incident
An SRE wrote a report script to analyze 250,000 AWS CloudWatch metric alarms and export anomaly metrics across 12 regions. When run in a container with a 2GB RAM limit, the container was killed by Linux OOMKilled (`Exit Code 137`).

### Root Cause
The script wrapped the entire pipeline in an array assignment:
```powershell
# BAD: Forces PowerShell to evaluate all 250,000 objects into memory at once!
$allAlarms = @(Get-CWMetricAlarm)
$filtered = $allAlarms | Where-Object { $_.StateValue -eq 'ALARM' }
```
Wrapping an expression in `@(...)` forces eager evaluation and buffers the entire stream in RAM.

### Architectural Solution: Pure Pipeline Streaming
```powershell
# GOOD: True streaming. Memory footprint remains under 85MB constant!
Get-CWMetricAlarm |
    Where-Object StateValue -eq 'ALARM' |
    Select-Object MetricName, Namespace, StateUpdatedTimestamp |
    Export-Csv -Path "./active_alarms.csv" -NoTypeInformation
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [06 - AWS Tools for PowerShell](./06-AWS-and-Multi-Cloud-Tools-for-PowerShell.md) | [README](./README.md) | [08 - Troubleshooting](./08-Troubleshooting.md) |
