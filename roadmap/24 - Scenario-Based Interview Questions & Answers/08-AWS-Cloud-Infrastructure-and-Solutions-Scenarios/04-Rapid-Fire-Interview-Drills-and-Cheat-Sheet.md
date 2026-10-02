# AWS Cloud Infrastructure & Solutions Scenarios: Rapid-Fire Drills & Master Cheat Sheet

> **Interview Focus:** Real-world incident response, root-cause analysis (RCA), diagnostic toolchains, and articulate candidate verbal pitches.

---

## 📌 Scenario 10: Multi-Region Active-Passive Failover Fails During Simulated DR Test

### 🚨 The Production Scenario
During an unannounced disaster recovery drill simulating us-east-1 failure, Route 53 DNS failover triggers to us-west-2, but users experience 500 errors because read-write database connections fail.

### 🔬 Root Cause Analysis (RCA) & Deep Mechanics
Route 53 DNS routed traffic to the standby region, but the secondary Aurora PostgreSQL cross-region read replica was not promoted to a standalone primary cluster, and application microservices in us-west-2 were still configured to write to the primary endpoint in the failed region.

### 🛠️ Step-by-Step Triage & Troubleshooting Workflow
- Inspect Route 53 Health Check status and DNS record TTLs (high TTLs cause clients to cache failed IPs).
- Promote the Aurora global database secondary cluster to standalone read-write mode.
- Trigger automated Systems Manager (SSM) Automation Document or AWS Fault Injection Simulator (FIS) runbook.
- Update AWS AppConfig or Parameter Store endpoints to point to the newly promoted local primary.
- Post-drill: implement AWS Route 53 Application Recovery Controller (ARC) for automated routing control with readiness checks.

### 💻 Exact Diagnostic & Remediation Commands
```bash
# Promote cross-region secondary cluster to primary
aws rds failover-global-cluster --global-cluster-identifier global-db --target-db-cluster-identifier-arn arn:aws:rds:us-west-2:123456789012:cluster:aurora-west

# Check Route 53 health check status
aws route53 get-health-check-status --health-check-id 12345678-abcd-12ab-34cd-56ef12345678

# Inspect Route 53 ARC routing control state
aws route53-recovery-control-config get-routing-control-state --routing-control-arn arn:aws:route53-recovery-control::123456789012:control/xxx

# Execute automated DR promotion runbook
aws ssm start-automation-execution --document-name 'AWSResilience-PromoteRDSReadReplica' --parameters 'ClusterId=aurora-west'

```

### 🗣️ Ideal Interview Candidate Answer (Verbal Pitch)
> "A DNS failover without database state failover leads directly to an outage. In a resilient multi-region architecture, Route 53 DNS switching must be orchestrated with Aurora Global Database failover. We utilize Route 53 Application Recovery Controller (ARC) to automate readiness checks and guarantee that database promotion, secret updates, and routing controls happen deterministically in under 60 seconds."

---

## ⚡ Rapid-Fire Diagnostic Cheat Sheet

| Symptom / Scenario | First Command to Run | Root Cause Hypothesis |
| :--- | :--- | :--- |
| **Scenario 1: Cross-Account S3 Bucket Access Fails with 403 Forbidden Despite Valid IAM Policy** | `aws sts get-caller-identity` | Cross-account S3 access requires explicit authorization in BOTH Account A's IAM ... |
| **Scenario 2: EKS Worker Nodes Fail to Join Cluster After Subnet Migration** | `aws ec2 get-console-output --instance-id i-0abcdef1234567890` | Worker nodes require route table connectivity to the EKS control plane API endpo... |
| **Scenario 3: ALB 504 Gateway Timeout Spikes During Traffic Peaks** | `aws elbv2 describe-load-balancers --names app-prod-alb --query 'LoadBalancers[0].LoadBalancerArn'` | HTTP 504 on an ALB indicates the load balancer established a connection to the t... |
| **Scenario 4: RDS Aurora PostgreSQL Replication Lag Explodes and Read Replicas Restart** | `aws rds describe-db-clusters --db-cluster-identifier aurora-prod-cluster --query 'DBClusters[0].CustomEndpoints'` | Even though Aurora uses a shared storage architecture where replicas read direct... |
| **Scenario 5: NAT Gateway Bandwidth & Data Transfer Costs Quadruple Unexpectedly** | `aws ec2 create-vpc-endpoint --vpc-id vpc-012345 --service-name com.amazonaws.us-east-1.s3 --route-table-ids rtb-private1 rtb-private2` | Backend applications running in private subnets were streaming large container i... |
| **Scenario 6: Auto Scaling Group Scaling Delays Cause Pod Eviction Cascades** | `kubectl get events -n kube-system --field-selector reason=FailedScheduling` | Cluster Autoscaler reacts only after pods enter 'Pending' status. Standard EC2 i... |
| **Scenario 7: CloudTrail Alerts Detect Leaked IAM Access Keys Executing Reconnaissance** | `aws iam update-access-key --access-key-id AKIAIOSFODNN7EXAMPLE --status Inactive --user-name compromised-dev` | A developer hardcoded AWS access keys into code that was pushed to GitHub. Autom... |
| **Scenario 8: AWS KMS Throttling Outages Impact Lambda and DynamoDB Encryption** | `aws cloudwatch get-metric-statistics --namespace AWS/KMS --metric-name ThrottledRequests --dimensions Name=KeyId,Value=1234abcd-12ab-34cd-56ef-1234567890ab --start-time $(date -u -v-1H +%FT%TZ) --end-time $(date -u +%FT%TZ) --period 60 --statistics Sum` | Each Lambda execution invoked KMS 'GenerateDataKey' or 'Decrypt' directly withou... |
| **Scenario 9: Transit Gateway Blackholing Traffic Following Route Table Update** | `aws ec2 search-transit-gateway-routes --transit-gateway-route-table-id tgw-rtb-012345 --filters 'Name=state,Values=blackhole'` | Transit Gateway route propagation was not enabled for the new VPC attachment, or... |
| **Scenario 10: Multi-Region Active-Passive Failover Fails During Simulated DR Test** | `aws rds failover-global-cluster --global-cluster-identifier global-db --target-db-cluster-identifier-arn arn:aws:rds:us-west-2:123456789012:cluster:aurora-west` | Route 53 DNS routed traffic to the standby region, but the secondary Aurora Post... |

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← AWS Cloud Infrastructure & Solutions Scenarios: Security & Disaster Recovery Scenarios](03-Security-and-Disaster-Recovery-Scenarios.md) | [Index](../../../README.md) | [Azure Cloud Infrastructure & Governance Scenarios: Production Incidents & Triage Scenarios →](../09-Azure-Cloud-Infrastructure-and-Governance-Scenarios/01-Production-Incidents-and-Triage-Scenarios.md) |

