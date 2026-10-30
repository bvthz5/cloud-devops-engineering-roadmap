# Azure Cloud Infrastructure & Governance Scenarios: Security & Disaster Recovery Scenarios

> **Interview Focus:** Real-world incident response, root-cause analysis (RCA), diagnostic toolchains, and articulate candidate verbal pitches.

---

## 📌 Scenario 7: Azure Virtual WAN Hub Router Reaches Throughput Limit During Database Backup Sync

### 🚨 The Production Scenario
Cross-region and branch-to-Azure communication through Azure Virtual WAN (vWAN) Hub experiences severe latency spikes and packet loss during nightly database backup synchronization.

### 🔬 Root Cause Analysis (RCA) & Deep Mechanics
The Virtual WAN Hub router was provisioned with standard scale units (2 scale units = 1 Gbps routing capacity). The database backup sync generated sustained 3.5 Gbps throughput, overwhelming the hub router's capacity and causing CPU throttling and packet drops across all spoke-to-spoke VNets.

### 🛠️ Step-by-Step Triage & Troubleshooting Workflow
- Check Virtual WAN Hub router metrics in Azure Monitor: 'Virtual Hub Routing Infrastructure Capacity' and 'Bytes Transferred'.
- Identify throughput saturation exceeding provisioned scale unit limits.
- Scale up the Virtual WAN Hub capacity scale units (e.g., from 2 scale units to 10 scale units = 5 Gbps capacity).
- Offload cross-region backup traffic to dedicated Azure Storage Private Endpoints via Global VNet Peering or AzCopy bandwidth throttling.
- Implement traffic shaping and scheduled backup off-peak windows.

### 💻 Exact Diagnostic & Remediation Commands
```bash
# Inspect Virtual WAN Hub scale units
az network vhub show -g rg-vwan -n vhub-eastus --query '{ScaleUnits:routingConfiguration, Provisioning:provisioningState}'

# Check real-time throughput consumption on vWAN router
az monitor metrics list --resource <VHUB_RESOURCE_ID> --metric 'HubRoutingThroughput' --interval PT1M

# Ensure standard SKU is enabled for scaling
az network vhub update -g rg-vwan -n vhub-eastus --sku Standard

# Increase vWAN Hub routing capacity to 10 scale units (5 Gbps)
az network vhub update -g rg-vwan -n vhub-eastus --capacity 10

```

### 🗣️ Ideal Interview Candidate Answer (Verbal Pitch)
> "Virtual WAN Hubs are managed SDN routers where 1 Scale Unit provides ~500 Mbps of routing capacity. Massive batch data flows like database backups can saturate router scale units, impacting all tenant traffic. I immediately scale the hub scale units via CLI, monitor 'HubRoutingThroughput' metrics, and optimize data architecture so heavy storage traffic bypasses the hub router using direct Private Endpoints."

---

## 📌 Scenario 8: Azure Front Door WAF Blocks Legitimate Customer API Payloads

### 🚨 The Production Scenario
Following the activation of the Default Rule Set (DRS 2.1) on Azure Front Door, customers report intermittent HTTP 403 Forbidden errors when submitting valid JSON requests containing SQL-like syntax or Markdown.

### 🔬 Root Cause Analysis (RCA) & Deep Mechanics
Azure Front Door WAF Core Rule Set detected patterns matching SQL Injection (e.g., Rule 942100) or Cross-Site Scripting (XSS) inside legitimate customer description fields, blocking requests in Prevention mode.

### 🛠️ Step-by-Step Triage & Troubleshooting Workflow
- Query Azure Front Door WAF diagnostic logs in Log Analytics to identify the exact triggered Rule ID, matched string, and client IP.
- Switch the WAF policy to 'Detection' mode temporarily if critical business flows are blocked.
- Add a WAF Rule Exclusion for the specific JSON request body parameter or specific rule ID.
- Create a Custom Rule with higher priority allowing verified customer API patterns.
- Validate with synthetic traffic and switch WAF back to 'Prevention' mode.

### 💻 Exact Diagnostic & Remediation Commands
```bash
# Query Log Analytics for WAF blocked requests
az network front-door waf-policy log-analytics-query --query 'AzureDiagnostics | where Category == "FrontdoorWebApplicationFirewallLog" | where action_s == "Block" | project TimeGenerated, ruleName_s, clientIP_s, requestUri_s, details_matches_s'

# Check WAF operational mode (Prevention vs Detection)
az network front-door waf-policy show -g rg-sec -n wafprod --query 'policySettings'

# Override false-positive rule to Allow or Disabled
az network front-door waf-policy managed-rules override add -g rg-sec --policy-name wafprod --rule-group-id SQLI --rule-id 942100 --action Allow

# Ensure WAF policy is active in Prevention mode
az network front-door waf-policy update -g rg-sec -n wafprod --mode Prevention

```

### 🗣️ Ideal Interview Candidate Answer (Verbal Pitch)
> "When a WAF rule triggers false positives, you never disable the entire WAF. I query the Front Door Log Analytics WAF logs to pinpoint the exact Rule ID and matched selector. Then I configure a targeted rule exclusion on that specific request body parameter or add an exclusion condition. This resolves the false positive for valid users while maintaining perimeter protection across the rest of the payload."

---

## 📌 Scenario 9: Azure SQL Database Long Transaction Log Fill-Up (Error 9002)

### 🚨 The Production Scenario
An Azure SQL Database in General Purpose tier halts all write operations, returning error: 'The transaction log for database is full due to ACTIVE_TRANSACTION (Error 9002)'.

### 🔬 Root Cause Analysis (RCA) & Deep Mechanics
An uncommitted transaction in a long-running batch job prevented the SQL Server transaction log from truncating during automated log backups, causing the log file to hit its max allocated storage limit.

### 🛠️ Step-by-Step Triage & Troubleshooting Workflow
- Query sys.databases to check 'log_reuse_wait_desc' to confirm the cause of log retention.
- Query sys.dm_tran_active_transactions and sys.dm_tran_session_transactions to find the blocking session ID.
- Kill the orphaned or rogue transaction session holding the log truncation lock.
- If transaction log remains full, scale up database vCore or storage allocation temporarily to restore write operations.
- Enforce batch commit chunking (e.g., 5,000 rows per transaction) to prevent massive single transaction logs.

### 💻 Exact Diagnostic & Remediation Commands
```bash
# Check transaction log truncation blocker
SELECT name, log_reuse_wait_desc FROM sys.databases WHERE name = DB_NAME();

# Identify session consuming log space
SELECT s.session_id, s.login_name, t.transaction_begin_time, td.database_transaction_log_bytes_used FROM sys.dm_tran_active_transactions t JOIN sys.dm_tran_database_transactions td ON t.transaction_id = td.transaction_id JOIN sys.dm_tran_session_transactions st ON t.transaction_id = st.transaction_id JOIN sys.dm_exec_sessions s ON st.session_id = s.session_id;

# Terminate rogue transaction to allow log truncation
KILL <BLOCKING_SESSION_ID>;

# Scale up database compute/storage capacity if needed
az sql db update -g rg-data -s sql-prod -n db-orders --capacity 8 --edition GeneralPurpose

```

### 🗣️ Ideal Interview Candidate Answer (Verbal Pitch)
> "In Azure SQL, log backups cannot truncate log space past the oldest active transaction. When error 9002 hits, I query sys.databases for log_reuse_wait_desc and join sys.dm_tran_database_transactions to identify the session ID holding the uncommitted transaction. After terminating the rogue session to release the log space, we refactor batch scripts to commit in controlled batches."

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← Azure Cloud Infrastructure & Governance Scenarios: Architecture & Scaling Scenarios](02-Architecture-and-Scaling-Scenarios.md) | [Index](../../../README.md) | [Azure Cloud Infrastructure & Governance Scenarios: Rapid-Fire Drills & Master Cheat Sheet →](04-Rapid-Fire-Interview-Drills-and-Cheat-Sheet.md) |

