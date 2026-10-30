# Enterprise System Design & Disaster Recovery Scenarios: Production Incidents & Triage Scenarios

> **Interview Focus:** Real-world incident response, root-cause analysis (RCA), diagnostic toolchains, and articulate candidate verbal pitches.

---

## 📌 Scenario 1: Active-Active Multi-Region Database Write Conflict Resolution

### 🚨 The Production Scenario
An enterprise runs an active-active architecture across US-East and EU-West. Two users update the same customer record simultaneously in both regions. The asynchronous cross-region replication stream collides, corrupting data integrity.

### 🔬 Root Cause Analysis (RCA) & Deep Mechanics
Multi-master active-active replication without deterministic conflict resolution. Both regions accepted writes independently, and naive 'Last-Write-Wins' (LWW) based on unsynchronized system wall clocks caused clock drift overwrites and lost updates.

### 🛠️ Step-by-Step Triage & Troubleshooting Workflow
- Transition to Conflict-Free Replicated Data Types (CRDTs) for additive and mergeable data structures.
- Implement Global Cell-Based Architecture: partition write ownership by geography or customer home region (e.g., EU users write strictly to EU primary) so writes never collide.
- If true multi-region write is mandatory, use globally distributed databases with TrueTime / Hybrid Logical Clocks (Google Cloud Spanner or CockroachDB).
- Configure Amazon DynamoDB Global Tables with strict versioning / conditional writes.
- Enforce transactional boundaries that isolate concurrent edits.

### 💻 Exact Diagnostic & Remediation Commands
```bash
# Inspect DynamoDB Global Table multi-region replication status
aws dynamodb describe-global-table --global-table-name Customers

# Check CockroachDB multi-region node status and range leases
cockroach node status --certs-dir=/certs

# Inspect Cloud Spanner multi-region configuration
gcloud spanner instances describe spanner-multi-region

# Inspect NTP / PTP clock synchronization and offset drift
chronyc tracking

```

### 🗣️ Ideal Interview Candidate Answer (Verbal Pitch)
> "True multi-region active-active writes are plagued by physics and clock drift. Last-Write-Wins based on system clocks will silently corrupt data. I solve this using Cell-Based Partitioning: users have a designated home region that handles their writes, eliminating cross-region collisions. For data that must be globally writable, we adopt Google Cloud Spanner or CockroachDB which use TrueTime and Raft consensus to guarantee serializable consistency."

---

## 📌 Scenario 2: Ransomware Infection Recovery - Cold Storage Air-Gapped Vault Restore

### 🚨 The Production Scenario
A sophisticated ransomware attack compromises enterprise domain controllers and encrypts all live production databases and primary cloud backups. The attacker demands $5 million in Bitcoin.

### 🔬 Root Cause Analysis (RCA) & Deep Mechanics
Backups were accessible through standard enterprise IAM credentials and stored on writable file shares within the same identity boundary, allowing the attacker to wipe snapshots before deploying ransomware.

### 🛠️ Step-by-Step Triage & Troubleshooting Workflow
- Declare P0 Security Emergency: sever all external internet connectivity and quarantine all compromised VPCs.
- Access the Air-Gapped Immutable Backup Vault located in a separate, isolated AWS account with AWS Backup Vault Lock in Compliance Mode.
- In Compliance Mode, even root accounts and compromised IAM credentials cannot delete or alter backups before the retention period expires.
- Provision a clean, sterile recovery VPC using automated Infrastructure-as-Code from a verified clean Git repository.
- Restore databases from immutable snapshots to the clean VPC and verify data integrity before opening traffic.

### 💻 Exact Diagnostic & Remediation Commands
```bash
# Verify status of AWS Backup Vault Lock in Compliance mode
aws backup describe-backup-vault --backup-vault-name AirGappedComplianceVault

# Initiate database restore from immutable recovery point
aws backup start-restore-job --recovery-point-arn arn:aws:backup:us-east-1:999999999999:recovery-point:... --metadata file://restore-config.json

# Spin up clean-room recovery VPC via immutable IaC
terraform apply -var-file=clean-room-env.tfvars

# Audit security findings to confirm threat eradication
aws guardduty get-findings --detector-id <DETECTOR_ID>

```

### 🗣️ Ideal Interview Candidate Answer (Verbal Pitch)
> "When ransomware strikes, attackers always target backups first. Our defense is an immutable, air-gapped backup vault in an isolated AWS account protected by AWS Backup Vault Lock in Compliance Mode. Once locked, not even an AWS account root user can delete or overwrite snapshots. We spin up a clean room via Terraform and restore production from immutable recovery points without ever paying a ransom."

---

## 📌 Scenario 3: Zero-Downtime Data Center to Cloud Database Migration (10TB Data)

### 🚨 The Production Scenario
An enterprise must migrate a 10TB mission-critical Oracle/PostgreSQL database from an on-premises data center to AWS RDS with a maximum allowed maintenance cutover window of 5 minutes.

### 🔬 Root Cause Analysis (RCA) & Deep Mechanics
A simple offline backup-and-restore would require 18 hours of downtime to dump, transfer, and import 10TB over the network, completely violating business continuity agreements.

### 🛠️ Step-by-Step Triage & Troubleshooting Workflow
- Deploy AWS Database Migration Service (DMS) or Debezium CDC for continuous change replication.
- Step 1: Perform full baseline initial load while the on-premises database continues serving live production traffic.
- Step 2: Enable Change Data Capture (CDC) to continuously capture and stream ongoing transactions from transaction logs (redo/WAL) to the RDS target.
- Step 3: Monitor DMS replication lag until target database is within 1 second of the source.
- Step 4: Cutover: place on-premises app in read-only mode for 60 seconds, wait for replication lag to reach 0, update DNS/connection strings to RDS, and open cloud application.

### 💻 Exact Diagnostic & Remediation Commands
```bash
# Inspect DMS replication task progress and status
aws dms describe-replication-tasks --filters Name=replication-task-id,Values=db-migration-task

# Test DMS connectivity to source and target endpoints
aws dms test-connection --replication-instance-arn arn:aws:dms:... --endpoint-arn arn:aws:dms:...

# Verify WAL generation rate on source database
SELECT pg_current_wal_lsn();

# Execute 60-second DNS cutover to RDS endpoint
aws route53 change-resource-record-sets --hosted-zone-id Z123 --change-batch file://cutover-dns.json

```

### 🗣️ Ideal Interview Candidate Answer (Verbal Pitch)
> "Zero-downtime database migration requires a two-phase CDC pattern. We execute a full baseline load while the source database is live, followed by continuous Change Data Capture streaming ongoing WAL transactions to the cloud target. When replication lag reaches sub-second parity, our cutover window takes under 2 minutes: pause source writes, verify zero lag, switch DNS to AWS RDS, and resume operations."

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← Multi-Cloud, Hybrid Cloud & FinOps Scenarios: Rapid-Fire Drills & Master Cheat Sheet](../19-Multi-Cloud-Hybrid-Cloud-and-FinOps-Scenarios/04-Rapid-Fire-Interview-Drills-and-Cheat-Sheet.md) | [Index](../../../README.md) | [Enterprise System Design & Disaster Recovery Scenarios: Architecture & Scaling Scenarios →](02-Architecture-and-Scaling-Scenarios.md) |

