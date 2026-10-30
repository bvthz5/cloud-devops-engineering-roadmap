# Multi-Cloud, Hybrid Cloud & FinOps Scenarios: Rapid-Fire Drills & Master Cheat Sheet

> **Interview Focus:** Real-world incident response, root-cause analysis (RCA), diagnostic toolchains, and articulate candidate verbal pitches.

---

## 📌 Scenario 10: Multi-Cloud Disaster Recovery Data Replication Discrepancy & Checksum Failures

### 🚨 The Production Scenario
During a cross-cloud disaster recovery test from AWS S3 to Google Cloud Storage (GCS), automated checksum audits report that 2% of synchronized files are corrupted or missing bytes.

### 🔬 Root Cause Analysis (RCA) & Deep Mechanics
AWS S3 calculates MD5 checksums only for single-part uploads; multipart uploads use a composite hash (`hash-N`). Google Cloud Storage uses CRC32C by default. When naive sync scripts compared S3 multipart ETag hashes directly against GCS MD5 hashes, valid files were flagged as corrupted or overwritten.

### 🛠️ Step-by-Step Triage & Troubleshooting Workflow
- Use native cross-cloud replication tools: GCP Storage Transfer Service or AWS DataSync.
- Standardize on end-to-end CRC32C checksum verification supported by both cloud providers.
- Re-run synchronization using GCP Storage Transfer Service with automated checksum validation enabled.
- Perform synthetic read-back verification on sample files.
- Document verified multi-cloud data sync runbooks with strict RPO tracking.

### 💻 Exact Diagnostic & Remediation Commands
```bash
# Initiate automated Google Cloud Storage Transfer Service from AWS S3
gcloud transfer jobs create s3://my-aws-bucket gs://my-gcp-bucket --name="cross-cloud-sync"

# Inspect status and throughput of cross-cloud data transfer
gcloud transfer operations list

# Calculate CRC32C checksum of replicated file in GCS
gsutil hash -c gs://my-gcp-bucket/file.tar.gz

# Retrieve S3 object CRC32C checksum
aws s3api head-object --bucket my-aws-bucket --key file.tar.gz --checksum-mode ENABLED

```

### 🗣️ Ideal Interview Candidate Answer (Verbal Pitch)
> "Cross-cloud checksum discrepancies often occur because S3 multipart ETags are not raw MD5 hashes. When verifying data integrity between AWS and GCP, you cannot compare ETags. I standardize on CRC32C checksum validation, which is supported natively across both AWS S3 and GCS, and execute the replication using GCP Storage Transfer Service for automated parallel integrity verification."

---

## ⚡ Rapid-Fire Diagnostic Cheat Sheet

| Symptom / Scenario | First Command to Run | Root Cause Hypothesis |
| :--- | :--- | :--- |
| **Scenario 1: Cross-Cloud Multi-Gigabit Egress Cost Explosion** | `aws ce get-cost-and-usage --time-period Start=2026-09-01,End=2026-09-30 --granularity MONTHLY --metrics "UnblendedCost" --group-by Type=DIMENSION,Key=USAGE_TYPE \| grep -i 'DataTransfer-Out'` | Public internet egress between clouds is charged at premium rates ($0.09/GB on A... |
| **Scenario 2: Spot Instance Eviction Storm Cascades Across Production Kubernetes Nodes** | `kubectl get poddisruptionbudgets -A` | Critical stateless production APIs were deployed 100% on Spot instances in a sin... |
| **Scenario 3: Orphaned Cloud Resources - Zombie EBS Volumes & Elastic IPs Bill Thousands** | `aws ec2 describe-volumes --filters Name=status,Values=available --query 'Volumes[*].[VolumeId,Size,CreateTime]' --output table` | CI/CD teardown pipelines destroyed EC2 instances but omitted the `delete_on_term... |
| **Scenario 4: AWS Savings Plans & Reserved Instances (RI) Underutilization & Mismatch** | `aws ce get-reservation-utilization --time-period Start=2026-09-01,End=2026-09-30` | The procurement team purchased rigid Standard EC2 Reserved Instances tied to a s... |
| **Scenario 5: Multi-Cloud Identity Federation (OIDC) Token Failure Locks Out CI/CD Pipelines** | `aws iam get-role --role-name GitHubActionsDeployRole --query 'Role.AssumeRolePolicyDocument'` | The GitHub organization renamed a repository or altered the default branch from ... |
| **Scenario 6: Cloud Cost Anomaly Detection - Rogue BigQuery Query Incurs $8,000 Bill** | `bq query --use_legacy_sql=false --dry_run 'SELECT * FROM `proj.dataset.clickstream`'` | BigQuery on-demand pricing charges $5 per TB scanned. The query executed against... |
| **Scenario 7: FinOps Tagging Governance Enforcement - Untagged Infrastructure Blocked at Inception** | `aws organizations describe-policy --policy-id p-tagpolicy123` | Engineers provisioned resources manually through the console or merged Terraform... |
| **Scenario 8: Hybrid Cloud VPN Gateway Bandwidth Saturation During Data Ingestion** | `aws ec2 describe-vpn-connections --vpn-connection-id vpn-0123456789abcdef0 --query 'VpnConnections[0].VgwTelemetry'` | A standard AWS IPsec VPN connection is limited to 1.25 Gbps per tunnel. The ETL ... |
| **Scenario 9: S3 Storage Class Lifecycle Misconfiguration Bills Petabytes at Standard Tier** | `aws s3api get-bucket-lifecycle-configuration --bucket corp-audit-logs` | No S3 Lifecycle policies were configured, or lifecycle rules failed to transitio... |
| **Scenario 10: Multi-Cloud Disaster Recovery Data Replication Discrepancy & Checksum Failures** | `gcloud transfer jobs create s3://my-aws-bucket gs://my-gcp-bucket --name="cross-cloud-sync"` | AWS S3 calculates MD5 checksums only for single-part uploads; multipart uploads ... |

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← Multi-Cloud, Hybrid Cloud & FinOps Scenarios: Security & Disaster Recovery Scenarios](03-Security-and-Disaster-Recovery-Scenarios.md) | [Index](../../../README.md) | [Enterprise System Design & Disaster Recovery Scenarios: Production Incidents & Triage Scenarios →](../20-Enterprise-System-Design-and-Disaster-Recovery-Scenarios/01-Production-Incidents-and-Triage-Scenarios.md) |

