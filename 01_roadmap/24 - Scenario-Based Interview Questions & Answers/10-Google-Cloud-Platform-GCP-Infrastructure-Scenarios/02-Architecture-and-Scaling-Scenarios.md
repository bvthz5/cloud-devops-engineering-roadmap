# Google Cloud Platform (GCP) Infrastructure Scenarios: Architecture & Scaling Scenarios

> **Interview Focus:** Real-world incident response, root-cause analysis (RCA), diagnostic toolchains, and articulate candidate verbal pitches.

---

## 📌 Scenario 4: BigQuery Slot Contention Causes Mission-Critical Dashboard Query Timeouts

### 🚨 The Production Scenario
At 9:00 AM daily, executive Looker dashboards fail to render, timing out after 120 seconds. BigQuery diagnostic logs show queries queued for minutes waiting for slot allocation.

### 🔬 Root Cause Analysis (RCA) & Deep Mechanics
All organizational workloads shared a single on-demand BigQuery slot pool or flat-rate reservation without workload isolation. Heavy automated ELT batch jobs ran concurrently with interactive BI queries, consuming all available slots and starving high-priority dashboard queries.

### 🛠️ Step-by-Step Triage & Troubleshooting Workflow
- Query BigQuery INFORMATION_SCHEMA.JOBS_BY_PROJECT to analyze slot allocation, job wait times, and heavy queries.
- Switch from unreserved on-demand querying to BigQuery Editions (Standard, Enterprise, Enterprise Plus).
- Create separate BigQuery Reservations: 'prod-bi' for dashboards and 'batch-elt' for data engineering.
- Configure Reservation Assignments linking BI user service accounts to the 'prod-bi' reservation.
- Enable Autoclass / slot autoscaling to absorb peak dashboard demand.

### 💻 Exact Diagnostic & Remediation Commands
```bash
# Identify top slot-consuming queries
SELECT project_id, job_id, total_slot_ms, query FROM `region-us`.INFORMATION_SCHEMA.JOBS_BY_PROJECT WHERE creation_time > TIMESTAMP_SUB(CURRENT_TIMESTAMP(), INTERVAL 1 HOUR) ORDER BY total_slot_ms DESC LIMIT 10;

# Create dedicated BI reservation
gcloud bigquery reservations create prod-bi-reservation --project=my-proj --location=US --slots=1000 --edition=ENTERPRISE

# Assign Looker service account to dedicated reservation
gcloud bigquery reservations assignments create --project=my-proj --location=US --reservation=prod-bi-reservation --job-type=QUERY --assignee-id=looker-sa@my-proj.iam.gserviceaccount.com --assignee-type=SERVICE_ACCOUNT

# Verify capacity commitments and slot counts
gcloud bigquery capacity-commitments list --project=my-proj --location=US

```

### 🗣️ Ideal Interview Candidate Answer (Verbal Pitch)
> "BigQuery query starvation happens when unmanaged batch pipelines consume all compute slots, leaving interactive BI queries waiting in queue. I solve this using BigQuery Reservations. By creating separate reservations for ELT jobs and executive dashboards and assigning service accounts accordingly, dashboards are guaranteed dedicated slots and SLA compliance regardless of background batch loads."

---

## 📌 Scenario 5: Cloud Spanner CPU Spikes to 100% Leading to High Read Latency

### 🚨 The Production Scenario
A Cloud Spanner database experiences a sudden increase in read/write latency from 5ms to 1200ms. High-priority CPU utilization in Cloud Monitoring exceeds 95%.

### 🔬 Root Cause Analysis (RCA) & Deep Mechanics
A newly deployed query lacked a secondary index on a frequently filtered column, triggering full table scans across millions of splits. Additionally, primary keys were monotonically increasing (auto-incrementing timestamps), causing write hotspotting on a single Split.

### 🛠️ Step-by-Step Triage & Troubleshooting Workflow
- Inspect Cloud Spanner Query Insights in GCP Console to identify queries with highest CPU consumption.
- Examine query execution plans for 'Filter' operations scanning the entire table instead of 'Index Scan'.
- Add a secondary index with STORING clauses to cover frequently queried columns without extra joins.
- Audit database schema: replace sequential integer or timestamp primary keys with UUID v4 or bit-reversed sequential values to prevent split hotspotting.
- Temporarily scale Spanner processing units / nodes to relieve immediate CPU pressure.

### 💻 Exact Diagnostic & Remediation Commands
```bash
# Check Spanner compute capacity
gcloud spanner instances describe spanner-prod --format='get(processingUnits,nodeCount)'

# Double Spanner compute capacity to alleviate pressure
gcloud spanner instances update spanner-prod --processing-units=2000

# Query Spanner top CPU consuming queries
SELECT query, avg_cpu_seconds, execution_count FROM spanner_sys.query_stat_top_minute ORDER BY avg_cpu_seconds DESC LIMIT 5;

# Inspect query execution plan for table scans
EXPLAIN SELECT user_id, order_id FROM Orders WHERE customer_email = 'test@example.com';

```

### 🗣️ Ideal Interview Candidate Answer (Verbal Pitch)
> "Spanner 100% CPU bottlenecks typically arise from two anti-patterns: unindexed queries forcing full table scans across splits, or monotonically increasing primary keys concentrating all writes on a single split leader. I use Query Insights to identify the rogue query, build covering secondary indexes, scale processing units temporarily, and enforce hash-distributed keys like UUIDv4 to balance load across all nodes."

---

## 📌 Scenario 6: VPC Service Controls (VPC-SC) Perimeter Violation Blocks Data Ingestion

### 🚨 The Production Scenario
A Dataflow pipeline running in project-a fails to write to a BigQuery dataset in project-b with: 'Security perimeter violation. Request blocked by VPC Service Controls'.

### 🔬 Root Cause Analysis (RCA) & Deep Mechanics
Project-b was placed inside a VPC Service Controls security perimeter to prevent data exfiltration. Because project-a and its Dataflow service account were outside the perimeter, all API calls crossing into project-b were blocked by default perimeter enforcement.

### 🛠️ Step-by-Step Triage & Troubleshooting Workflow
- Query Cloud Logging for 'servicePerimeterName' to extract the violation record and caller identity.
- Determine whether project-a should be added into the same service perimeter.
- If project-a must remain separate, define a VPC Service Controls Ingress/Egress Rule permitting Dataflow's identity and BigQuery API method.
- Test rule configuration in VPC-SC 'Dry Run' mode to verify no unintended violations occur.
- Promote dry-run rules to enforced mode.

### 💻 Exact Diagnostic & Remediation Commands
```bash
# Inspect VPC-SC violation details in audit logs
gcloud logging read 'protoPayload.metadata.violationReason="SECURITY_PERIMETER_VIOLATION"' --limit 5 --format json

# View existing perimeter members and rules
gcloud access-context-manager perimeters describe prod_perimeter --policy=123456789012

# Test adding project in dry-run mode
gcloud access-context-manager perimeters dry-run update prod_perimeter --policy=123456789012 --add-resources=projects/987654321098

# Enforce dry-run configuration to production perimeter
gcloud access-context-manager perimeters dry-run enforce prod_perimeter --policy=123456789012

```

### 🗣️ Ideal Interview Candidate Answer (Verbal Pitch)
> "VPC Service Controls create perimeter fences around Google Cloud APIs to prevent data exfiltration. When a perimeter violation occurs, I examine Cloud Audit Logs to extract the caller principal, source IP/VPC, and target service. I configure granular Ingress and Egress rules specifying the exact Service Account and API method rather than poking broad holes, and always validate in Dry-Run mode before enforcing."

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← Google Cloud Platform (GCP) Infrastructure Scenarios: Production Incidents & Triage Scenarios](01-Production-Incidents-and-Triage-Scenarios.md) | [Index](../../../README.md) | [Google Cloud Platform (GCP) Infrastructure Scenarios: Security & Disaster Recovery Scenarios →](03-Security-and-Disaster-Recovery-Scenarios.md) |

