# Google Cloud Platform (GCP) Infrastructure Scenarios: Rapid-Fire Drills & Master Cheat Sheet

> **Interview Focus:** Real-world incident response, root-cause analysis (RCA), diagnostic toolchains, and articulate candidate verbal pitches.

---

## 📌 Scenario 10: Asynchronous Cloud Pub/Sub Dead Letter Queue Accumulation

### 🚨 The Production Scenario
A Cloud Pub/Sub dead letter topic receives 10,000 messages per minute. Downstream subscriber services report high latency, and order processing workflows fail silently.

### 🔬 Root Cause Analysis (RCA) & Deep Mechanics
The primary Pub/Sub subscriber service encountered uncaught serialization exceptions when parsing malformed JSON payloads from a partner API. Because the subscriber returned HTTP 500 or failed to acknowledge, Pub/Sub retried 5 times and forwarded the poison messages to the Dead Letter Queue (DLQ).

### 🛠️ Step-by-Step Triage & Troubleshooting Workflow
- Inspect subscriber service error logs in Cloud Logging to extract stack trace and sample payload.
- Pull a sample message from the Dead Letter Subscription to inspect the malformed JSON schema.
- Deploy an emergency hotfix to the subscriber service with safe schema validation and graceful error handling.
- Create a replay worker or Cloud Function to drain, fix, and republish valid messages from the DLQ back to the main topic.
- Add alert policies on Pub/Sub 'Dead Letter Message Count' and 'Oldest Unacked Message Age'.

### 💻 Exact Diagnostic & Remediation Commands
```bash
# Inspect sample poison message from DLQ subscription
gcloud pubsub subscriptions pull dlq-sub --limit=5 --auto-ack=false

# Inspect max delivery attempts and dead letter topic settings
gcloud pubsub subscriptions describe order-service-sub --format='get(deadLetterPolicy)'

# Read subscriber service exception logs
gcloud logging read 'resource.type="cloud_run_revision" AND severity>=ERROR' --limit 10

# Test republishing fixed message
gcloud pubsub topics publish order-main-topic --message='{"order_id": "1234", "valid": true}'

```

### 🗣️ Ideal Interview Candidate Answer (Verbal Pitch)
> "Dead letter queues protect subscriber pipelines from poison message crash loops. When DLQ volume spikes, I pull a sample message without auto-acknowledging to inspect the malformed payload, cross-referencing subscriber logs for the exact parsing exception. Once the application schema parser is patched and deployed, I run a dead-letter replay pipeline to safely re-inject sanitized messages back into the primary topic."

---

## ⚡ Rapid-Fire Diagnostic Cheat Sheet

| Symptom / Scenario | First Command to Run | Root Cause Hypothesis |
| :--- | :--- | :--- |
| **Scenario 1: GKE Workload Identity Fails with 'Could Not Refresh Access Token: 403 Forbidden'** | `gcloud container clusters describe gke-prod --zone us-central1-a --query 'workloadIdentityConfig'` | Workload Identity requires three-way binding: GKE cluster must have Workload Ide... |
| **Scenario 2: Cloud Load Balancing 502 Bad Gateway with 'backend_timeout' or 'failed_to_connect_to_backend'** | `gcloud logging read 'resource.type="http_load_balancer" AND httpRequest.status=502' --limit 10 --format json` | GCP Cloud Load Balancer default backend service timeout is 30 seconds. Heavy bac... |
| **Scenario 3: Private Google Access Failure Blocks Cloud Storage Downloads from Private Compute VMs** | `gcloud compute networks subnets describe snet-private-backend --region=us-central1 --format='get(privateIpGoogleAccess)'` | The VPC subnet hosting the instances did not have 'Private Google Access' enable... |
| **Scenario 4: BigQuery Slot Contention Causes Mission-Critical Dashboard Query Timeouts** | `SELECT project_id, job_id, total_slot_ms, query FROM `region-us`.INFORMATION_SCHEMA.JOBS_BY_PROJECT WHERE creation_time > TIMESTAMP_SUB(CURRENT_TIMESTAMP(), INTERVAL 1 HOUR) ORDER BY total_slot_ms DESC LIMIT 10;` | All organizational workloads shared a single on-demand BigQuery slot pool or fla... |
| **Scenario 5: Cloud Spanner CPU Spikes to 100% Leading to High Read Latency** | `gcloud spanner instances describe spanner-prod --format='get(processingUnits,nodeCount)'` | A newly deployed query lacked a secondary index on a frequently filtered column,... |
| **Scenario 6: VPC Service Controls (VPC-SC) Perimeter Violation Blocks Data Ingestion** | `gcloud logging read 'protoPayload.metadata.violationReason="SECURITY_PERIMETER_VIOLATION"' --limit 5 --format json` | Project-b was placed inside a VPC Service Controls security perimeter to prevent... |
| **Scenario 7: Cloud Run Cold Starts Cause HTTP 504 Timeouts for Real-Time Mobile Clients** | `gcloud run services describe ml-inference --region us-central1 --format='get(spec.template.metadata.annotations)'` | Cloud Run scaled instance count to 0 during idle periods. When a new request arr... |
| **Scenario 8: Cloud SQL Primary High Disk Utilization Emergency (98% Full)** | `gcloud sql instances describe sql-prod --format='get(settings.dataDiskSizeGb,settings.dataDiskType)'` | Automatic storage increase was disabled or capped, and large binary logs (binlog... |
| **Scenario 9: Cloud Build Pipeline Pipeline Timeout Due to Ephemeral Disk Exhaustion** | `gcloud builds submit --config=cloudbuild.yaml --substitutions=_DISK_SIZE=200GB` | Standard Cloud Build worker pools execute on VMs with default 100 GB boot disks.... |
| **Scenario 10: Asynchronous Cloud Pub/Sub Dead Letter Queue Accumulation** | `gcloud pubsub subscriptions pull dlq-sub --limit=5 --auto-ack=false` | The primary Pub/Sub subscriber service encountered uncaught serialization except... |

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← Google Cloud Platform (GCP) Infrastructure Scenarios: Security & Disaster Recovery Scenarios](03-Security-and-Disaster-Recovery-Scenarios.md) | [Index](../../../README.md) | [CI/CD Pipelines & Automation Scenarios: Production Incidents & Triage Scenarios →](../11-CICD-Pipelines-and-Automation-Scenarios/01-Production-Incidents-and-Triage-Scenarios.md) |

