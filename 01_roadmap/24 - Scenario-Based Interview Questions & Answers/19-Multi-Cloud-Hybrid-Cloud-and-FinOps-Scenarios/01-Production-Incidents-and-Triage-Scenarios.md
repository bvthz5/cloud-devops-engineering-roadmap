# Multi-Cloud, Hybrid Cloud & FinOps Scenarios: Production Incidents & Triage Scenarios

> **Interview Focus:** Real-world incident response, root-cause analysis (RCA), diagnostic toolchains, and articulate candidate verbal pitches.

---

## 📌 Scenario 1: Cross-Cloud Multi-Gigabit Egress Cost Explosion

### 🚨 The Production Scenario
An enterprise operating workloads in both AWS and GCP discovers a $45,000/month surprise bill for data egress. Microservices in AWS EKS were streaming raw telemetry and database snapshots to GCP BigQuery over public internet gateways.

### 🔬 Root Cause Analysis (RCA) & Deep Mechanics
Public internet egress between clouds is charged at premium rates ($0.09/GB on AWS). Streaming raw, uncompressed high-frequency data across cloud boundaries without compression or private interconnects generated massive billing overages.

### 🛠️ Step-by-Step Triage & Troubleshooting Workflow
- Analyze egress traffic by source and destination using AWS Cost Anomaly Detection and VPC Flow Logs.
- Deploy Megaport or Equinix Cloud Exchange dedicated interconnect connecting AWS Direct Connect and GCP Cloud Interconnect directly at Layer 2/3 (cutting egress rates by 50-70%).
- Compress and batch data before transit (e.g., Parquet / Zstandard) rather than streaming raw JSON.
- Architectural realignment: move data processing pipelines to run locally in the cloud where the data resides, transferring only aggregated summaries.
- Configure budget alerts with AWS Budgets and GCP Billing Alerts.

### 💻 Exact Diagnostic & Remediation Commands
```bash
# Identify AWS egress billing lines
aws ce get-cost-and-usage --time-period Start=2026-09-01,End=2026-09-30 --granularity MONTHLY --metrics "UnblendedCost" --group-by Type=DIMENSION,Key=USAGE_TYPE | grep -i 'DataTransfer-Out'

# Inspect Direct Connect virtual interface status
aws directconnect describe-virtual-interfaces

# Inspect GCP Cloud Interconnect status
gcloud compute interconnects list

# Inspect cross-cloud network packet stream
tcpdump -i eth0 -n 'tcp and (dst net 35.0.0.0/8)' -c 10

```

### 🗣️ Ideal Interview Candidate Answer (Verbal Pitch)
> "Cross-cloud data egress over public internet is a major FinOps anti-pattern. I resolve this with three strategies: first, batch and compress data with Parquet/Zstandard to reduce byte volume by 80%; second, connect clouds using Megaport Cloud Router over private Direct Connect/Interconnect to cut per-GB rates in half; third, co-locate compute with data so only final aggregates cross the cloud perimeter."

---

## 📌 Scenario 2: Spot Instance Eviction Storm Cascades Across Production Kubernetes Nodes

### 🚨 The Production Scenario
AWS reclaims 80% of an enterprise's EC2 Spot instances within 5 minutes due to market capacity pressure. EKS clusters suffer massive pod eviction storms, dropping user requests and failing 50% of transactions.

### 🔬 Root Cause Analysis (RCA) & Deep Mechanics
Critical stateless production APIs were deployed 100% on Spot instances in a single instance family (e.g., only `m5.large`) in a single availability zone, without Spot diversification or graceful node termination handling.

### 🛠️ Step-by-Step Triage & Troubleshooting Workflow
- Deploy AWS Node Termination Handler or Karpenter to intercept AWS 2-minute Spot Interruption Warnings via EventBridge.
- Cordon and drain nodes immediately upon receiving the 2-minute warning, cordoning the node and evicting pods gracefully.
- Diversify Spot instance pools: configure Karpenter or ASG to select across 15+ instance types (c5, c6i, m5, m6i, r5) and multiple availability zones.
- Enforce architecture rule: production workloads run on a baseline of On-Demand / Savings Plans instances (e.g., 40%) and use Spot exclusively for autoscaling burst capacity (60%).
- Configure PodDisruptionBudgets (PDB) to ensure minimum pod availability during evictions.

### 💻 Exact Diagnostic & Remediation Commands
```bash
# Audit active PodDisruptionBudgets across cluster
kubectl get poddisruptionbudgets -A

# Inspect Karpenter instance family diversification
kubectl get nodepool default -o yaml | grep instance-family

# Inspect real-time Spot market pricing and volatility
aws ec2 describe-spot-price-history --instance-types m5.large c5.large --start-time $(date -u -v-1H +%FT%TZ)

# Check node termination interruption events
kubectl describe node <SPOT_NODE> | grep -i 'karpenter.sh/interruption'

```

### 🗣️ Ideal Interview Candidate Answer (Verbal Pitch)
> "Spot instances can save 70%, but running critical production 100% on a single Spot instance family invites catastrophic evictions. I implement a resilient architecture using Karpenter with broad instance diversification across 20+ instance types and 3 AZs. We maintain a 40% On-Demand baseline for core stability and intercept the AWS 2-minute interruption warning to drain pods before the instance terminates."

---

## 📌 Scenario 3: Orphaned Cloud Resources - Zombie EBS Volumes & Elastic IPs Bill Thousands

### 🚨 The Production Scenario
A FinOps audit discovers an organization is spending $8,500 every month on unattached AWS EBS volumes, idle Elastic Load Balancers, and unallocated Elastic IPs left behind by destroyed staging environments.

### 🔬 Root Cause Analysis (RCA) & Deep Mechanics
CI/CD teardown pipelines destroyed EC2 instances but omitted the `delete_on_termination` flag for secondary EBS volumes, and Terraform destroy commands were skipped or failed midway, leaving orphaned infrastructure behind.

### 🛠️ Step-by-Step Triage & Troubleshooting Workflow
- Scan for orphaned resources using open-source FinOps tools like `cloud-custodian` or `aws-nuke`.
- Audit unattached EBS volumes: create snapshots and delete unattached volumes older than 7 days.
- Release unallocated Elastic IPs and delete idle load balancers with zero registered targets.
- Enforce Infrastructure-as-Code lifecycle policies and automated ephemeral environment TTL destruction (e.g., auto-destroy staging namespaces and clusters nightly).
- Deploy automated Cloud Custodian policies to continuously clean orphaned assets.

### 💻 Exact Diagnostic & Remediation Commands
```bash
# List all unattached zombie EBS volumes
aws ec2 describe-volumes --filters Name=status,Values=available --query 'Volumes[*].[VolumeId,Size,CreateTime]' --output table

# List all unallocated Elastic IPs incurring idle fees
aws ec2 describe-addresses --query 'Addresses[?NetworkInterfaceId==null].[PublicIp,AllocationId]' --output table

# Execute Cloud Custodian automated orphan resource remediation policy
custodian run --output-dir=. custodian-cleanup.yml

# Audit load balancers across account
aws elbv2 describe-load-balancers --query 'LoadBalancers[*].[LoadBalancerName,LoadBalancerArn]' --output table

```

### 🗣️ Ideal Interview Candidate Answer (Verbal Pitch)
> "Orphaned infrastructure is silent cloud waste. I run Cloud Custodian policies that continuously audit and purge unattached EBS volumes, empty ALBs, and unassociated Elastic IPs. In staging environments, we enforce automated ephemeral TTLs: any test environment provisioned in CI is scheduled for automated destruction after 8 hours, eliminating abandoned infrastructure entirely."

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← AI for DevOps, AIOps & LLMOps Scenarios: Rapid-Fire Drills & Master Cheat Sheet](../18-AI-for-DevOps-AIOps-and-LLMOps-Scenarios/04-Rapid-Fire-Interview-Drills-and-Cheat-Sheet.md) | [Index](../../../README.md) | [Multi-Cloud, Hybrid Cloud & FinOps Scenarios: Architecture & Scaling Scenarios →](02-Architecture-and-Scaling-Scenarios.md) |

