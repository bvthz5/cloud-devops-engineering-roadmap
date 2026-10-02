# AWS Cloud Infrastructure & Solutions Scenarios: Architecture & Scaling Scenarios

> **Interview Focus:** Real-world incident response, root-cause analysis (RCA), diagnostic toolchains, and articulate candidate verbal pitches.

---

## 📌 Scenario 4: RDS Aurora PostgreSQL Replication Lag Explodes and Read Replicas Restart

### 🚨 The Production Scenario
Under heavy batch writes, Aurora Read Replicas experience ballooning replication lag, eventually causing read queries to fail and replicas to reboot spontaneously.

### 🔬 Root Cause Analysis (RCA) & Deep Mechanics
Even though Aurora uses a shared storage architecture where replicas read directly from the Cluster Volume, replicas must process WAL redo log records in local memory to update their local buffer cache. High write throughput or long-running analytical queries on the replica can hold buffer pin locks, causing replication lag or OOM reboots due to WAL queue buffer exhaustion.

### 🛠️ Step-by-Step Triage & Troubleshooting Workflow
- Inspect CloudWatch metrics: 'AuroraReplicaLag', 'BufferCacheHitRatio', and 'FreeableMemory'.
- Identify long-running read queries on the replica blocking WAL replay using pg_stat_activity.
- Configure 'max_standby_streaming_delay' or 'hot_standby_feedback' in Aurora DB cluster parameter groups.
- Distribute analytical queries to a dedicated Aurora custom endpoint away from real-time read replicas.
- Optimize batch write operations into smaller chunks to avoid overwhelming the replication log buffer.

### 💻 Exact Diagnostic & Remediation Commands
```bash
# Inspect custom read endpoints
aws rds describe-db-clusters --db-cluster-identifier aurora-prod-cluster --query 'DBClusters[0].CustomEndpoints'

# Find long running queries blocking WAL replay on replica
SELECT pid, now() - query_start AS duration, query, state FROM pg_stat_activity WHERE state != 'idle' ORDER BY duration DESC;

# Check standby streaming delay threshold
SHOW max_standby_streaming_delay;

# Adjust streaming delay threshold
aws rds modify-db-cluster-parameter-group --db-cluster-parameter-group-name custom-aurora --parameters 'ParameterName=max_standby_streaming_delay,ParameterValue=30000,ApplyMethod=immediate'

```

### 🗣️ Ideal Interview Candidate Answer (Verbal Pitch)
> "Aurora replication lag differs from standard RDS because storage is shared. Lag occurs when the replica's local buffer cache cannot apply storage redo logs fast enough, often blocked by long-running read transactions. I isolate heavy analytics to a dedicated custom endpoint, tune max_standby_streaming_delay to cancel queries exceeding SLAs, and throttle batch ingest pipelines."

---

## 📌 Scenario 5: NAT Gateway Bandwidth & Data Transfer Costs Quadruple Unexpectedly

### 🚨 The Production Scenario
AWS Cost Explorer reports an unexpected 400% spike in NAT Gateway charges for 'BytesProcessed-In' and 'BytesProcessed-Out' in a private VPC holding microservices.

### 🔬 Root Cause Analysis (RCA) & Deep Mechanics
Backend applications running in private subnets were streaming large container images from ECR and reading terabytes of backups from Amazon S3 through the default 0.0.0.0/0 route via NAT Gateway, incurring both NAT Gateway hourly data transfer fees ($0.045/GB) and internet egress fees.

### 🛠️ Step-by-Step Triage & Troubleshooting Workflow
- Enable and analyze VPC Flow Logs with CloudWatch Logs Insights or Athena to identify top talkers by source/destination IP and byte volume.
- Map top destination public IPs to AWS services (S3, ECR, DynamoDB) using AWS IP ranges.
- Provision free S3 and DynamoDB Gateway VPC Endpoints and associate them with the private subnet route tables.
- Provision Interface VPC Endpoints (AWS PrivateLink) for ECR (ecr.api, ecr.dkr) and CloudWatch Logs.
- Verify route table priority: Gateway endpoints automatically inject more specific routes (pl-xxxxxxxx) taking precedence over the default 0.0.0.0/0 NAT route.

### 💻 Exact Diagnostic & Remediation Commands
```bash
# Create S3 Gateway Endpoint to bypass NAT Gateway
aws ec2 create-vpc-endpoint --vpc-id vpc-012345 --service-name com.amazonaws.us-east-1.s3 --route-table-ids rtb-private1 rtb-private2

# Verify prefix list route injected for S3
aws ec2 describe-route-tables --route-table-ids rtb-private1 --query 'RouteTables[0].Routes'

# Create ECR Docker Interface Endpoint
aws ec2 create-vpc-endpoint --vpc-id vpc-012345 --service-name com.amazonaws.us-east-1.ecr.dkr --vpc-endpoint-type Interface --subnet-ids subnet-priv1 subnet-priv2

# Analyze top flow log destinations
aws logs start-query --log-group-name vpc-flow-logs --start-time 1600000000 --end-time 1600003600 --query-string 'stats sum(bytes) as TotalBytes by dstAddr | sort TotalBytes desc | limit 10'

```

### 🗣️ Ideal Interview Candidate Answer (Verbal Pitch)
> "NAT Gateway costs skyrocket when traffic destined for AWS services leaves the private VPC through the NAT gateway. I analyze VPC Flow Logs using Athena to identify high-volume destination IPs. By provisioning free S3 Gateway Endpoints and PrivateLink endpoints for ECR and CloudWatch, internal AWS traffic stays on the AWS private backbone, cutting NAT data transfer costs to zero."

---

## 📌 Scenario 6: Auto Scaling Group Scaling Delays Cause Pod Eviction Cascades

### 🚨 The Production Scenario
During a sudden 10x traffic spike, an EKS cluster with Cluster Autoscaler fails to scale fast enough. Nodes become memory-pressured, triggering pod evictions, cascading failures, and 503 service outages.

### 🔬 Root Cause Analysis (RCA) & Deep Mechanics
Cluster Autoscaler reacts only after pods enter 'Pending' status. Standard EC2 instances take 2-4 minutes to initialize, attach EBS volumes, run cloud-init, and register with the Kubernetes API. In fast spikes, this delay leads to overwhelming existing nodes.

### 🛠️ Step-by-Step Triage & Troubleshooting Workflow
- Switch from legacy Cluster Autoscaler to Karpenter for fast, node-level provisioning directly via AWS Fleet APIs.
- Implement 'Overprovisioning / Pause Pods' with negative priority classes to maintain a warm buffer of compute capacity.
- Configure Karpenter provisioners with diverse instance types (c5.large, c6i.large, m5.large) across multiple AZs to avoid EC2 capacity pool exhaustion.
- Tune Container Image Pull times using containerd image pre-warming or AWS Bottlerocket OS.
- Align Horizontal Pod Autoscaler (HPA) metrics with predictive scaling or lower target utilization thresholds (65%).

### 💻 Exact Diagnostic & Remediation Commands
```bash
# Check pods blocked awaiting compute nodes
kubectl get events -n kube-system --field-selector reason=FailedScheduling

# Inspect Karpenter NodePool configuration and constraints
kubectl get nodepool -o yaml

# Verify instance type candidate specifications
aws ec2 describe-instance-types --instance-types c5.xlarge c6i.xlarge --query 'InstanceTypes[*].[InstanceType,MemoryInfo.SizeInMiB]'

# Inspect real-time node capacity vs consumption
kubectl top nodes && kubectl top pods -A

```

### 🗣️ Ideal Interview Candidate Answer (Verbal Pitch)
> "Reactive scaling with Cluster Autoscaler has a 3-5 minute lag. I solve this using Karpenter, which provisions optimal EC2 instances in under 45 seconds directly through EC2 APIs without ASG overhead. To guarantee zero-wait headroom, I run low-priority 'balloon' pause pods that get immediately evicted when production pods schedule, giving real workloads instant compute."

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← AWS Cloud Infrastructure & Solutions Scenarios: Production Incidents & Triage Scenarios](01-Production-Incidents-and-Triage-Scenarios.md) | [Index](../../../README.md) | [AWS Cloud Infrastructure & Solutions Scenarios: Security & Disaster Recovery Scenarios →](03-Security-and-Disaster-Recovery-Scenarios.md) |

