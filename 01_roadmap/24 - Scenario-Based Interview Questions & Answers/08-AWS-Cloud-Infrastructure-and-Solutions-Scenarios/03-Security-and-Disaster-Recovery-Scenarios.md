# AWS Cloud Infrastructure & Solutions Scenarios: Security & Disaster Recovery Scenarios

> **Interview Focus:** Real-world incident response, root-cause analysis (RCA), diagnostic toolchains, and articulate candidate verbal pitches.

---

## 📌 Scenario 7: CloudTrail Alerts Detect Leaked IAM Access Keys Executing Reconnaissance

### 🚨 The Production Scenario
AWS GuardDuty fires high-severity alerts: 'Recon:IAMUser/NetworkPermissions' and 'UnauthorizedAccess:IAMUser/InstanceCredentialExfiltration'. An engineer accidentally committed long-lived access keys to a public GitHub repository.

### 🔬 Root Cause Analysis (RCA) & Deep Mechanics
A developer hardcoded AWS access keys into code that was pushed to GitHub. Automated threat actor scrapers detected the keys within seconds and began executing API enumeration ('DescribeInstances', 'ListBuckets', 'CreateUser') to discover privilege escalation vectors.

### 🛠️ Step-by-Step Triage & Troubleshooting Workflow
- Immediately deactivate or delete the compromised IAM Access Key via AWS CLI.
- Attach an explicit inline deny policy ('DenyAll') to the compromised IAM user to instantly sever active sessions.
- Query CloudTrail event history to audit all API calls initiated by the compromised Access Key ID in the last 24 hours.
- Inspect and terminate any unauthorized EC2 instances (often crypto-miners) or backdoor IAM roles created by the attacker.
- Rotate all corporate credentials and enforce AWS IAM Roles with short-lived STS tokens instead of static access keys.

### 💻 Exact Diagnostic & Remediation Commands
```bash
# Immediately deactivate compromised access key
aws iam update-access-key --access-key-id AKIAIOSFODNN7EXAMPLE --status Inactive --user-name compromised-dev

# Apply emergency quarantine DenyAll policy
aws iam put-user-policy --user-name compromised-dev --policy-name IncidentContainmentDeny --policy-document '{"Version":"2012-10-17","Statement":[{"Effect":"Deny","Action":"*","Resource":"*"}]}'

# Audit attacker API activity across CloudTrail
aws cloudtrail lookup-events --lookup-attributes AttributeKey=AccessKeyId,AttributeValue=AKIAIOSFODNN7EXAMPLE --start-time $(date -u -v-24H +%s) --end-time $(date -u +%s)

# Check for rogue crypto-mining instances launched
aws ec2 describe-instances --filters 'Name=instance-state-name,Values=running' --query 'Reservations[*].Instances[*].[InstanceId,LaunchTime,InstanceType]'

```

### 🗣️ Ideal Interview Candidate Answer (Verbal Pitch)
> "The moment a key leak is detected, containment must take under 60 seconds: deactivate the key and attach an explicit DenyAll inline policy. Then I audit CloudTrail for all calls by that access key to identify unauthorized infrastructure, terminate rogue resources, and check for new IAM users or backdoors. Long term, we ban static IAM keys in favor of IAM Identity Center (SSO), OIDC for CI/CD, and GitHub Secret Scanning."

---

## 📌 Scenario 8: AWS KMS Throttling Outages Impact Lambda and DynamoDB Encryption

### 🚨 The Production Scenario
During high-throughput event processing, AWS Lambda functions fail to write to DynamoDB with 'KMS.ThrottlingException: Rate exceeded'. Message queues back up across SQS.

### 🔬 Root Cause Analysis (RCA) & Deep Mechanics
Each Lambda execution invoked KMS 'GenerateDataKey' or 'Decrypt' directly without client-side caching. AWS KMS has per-account regional request rate quotas (e.g., 10,000 to 50,000 req/sec). Exceeding this quota triggers throttling across all services relying on that KMS key.

### 🛠️ Step-by-Step Triage & Troubleshooting Workflow
- Check CloudWatch metrics for AWS KMS 'ThrottledRequests' by KeyId.
- Implement AWS Encryption SDK with Data Key Caching in Lambda to reuse data keys across function invocations safely.
- Switch to AWS KMS Customer Managed Keys with DynamoDB table encryption caching or use DynamoDB service-owned keys if custom rotation is not required.
- Request a KMS TPS quota increase via AWS Service Quotas.
- Implement exponential backoff and jitter in the SDK retry logic.

### 💻 Exact Diagnostic & Remediation Commands
```bash
# Measure KMS throttling rate
aws cloudwatch get-metric-statistics --namespace AWS/KMS --metric-name ThrottledRequests --dimensions Name=KeyId,Value=1234abcd-12ab-34cd-56ef-1234567890ab --start-time $(date -u -v-1H +%FT%TZ) --end-time $(date -u +%FT%TZ) --period 60 --statistics Sum

# Check regional KMS cryptographic operations quota
aws service-quotas get-service-quota --service-code kms --quota-code L-E75D8337

# Request KMS TPS quota increase
aws service-quotas request-service-quota-increase --service-code kms --quota-code L-E75D8337 --desired-value 30000

# Verify key state and multi-region status
aws kms describe-key --key-id 1234abcd-12ab-34cd-56ef-1234567890ab

```

### 🗣️ Ideal Interview Candidate Answer (Verbal Pitch)
> "KMS throttling occurs when high-concurrency workloads invoke cryptographic API calls for every record. I resolve this by implementing Data Key Caching using the AWS Encryption SDK, which securely reuses cached keys across Lambda warm containers. I also configure exponential backoff with full jitter and request proactive Service Quota increases for high-throughput regions."

---

## 📌 Scenario 9: Transit Gateway Blackholing Traffic Following Route Table Update

### 🚨 The Production Scenario
After adding a new VPC attachment to an AWS Transit Gateway (TGW), traffic between existing VPC A and VPC B drops to 0%. Packet captures show packets leaving VPC A but never arriving at VPC B.

### 🔬 Root Cause Analysis (RCA) & Deep Mechanics
Transit Gateway route propagation was not enabled for the new VPC attachment, or the Transit Gateway route table contained an overriding static route or an incorrect blackhole route pointing to a deleted attachment.

### 🛠️ Step-by-Step Triage & Troubleshooting Workflow
- Check Transit Gateway route tables for 'Blackhole' status routes.
- Verify TGW Route Table Associations and Route Table Propagations for both VPC A and VPC B attachments.
- Inspect VPC subnet route tables in both VPCs: verify route for remote CIDR points to 'tgw-xxxx'.
- Verify Security Groups and Network ACLs (NACLs) allow traffic both inbound and outbound on both sides.
- Use VPC Reachability Analyzer or Network Access Analyzer to test deterministic path reachability.

### 💻 Exact Diagnostic & Remediation Commands
```bash
# Find blackhole routes in Transit Gateway
aws ec2 search-transit-gateway-routes --transit-gateway-route-table-id tgw-rtb-012345 --filters 'Name=state,Values=blackhole'

# Verify VPC route propagation status
aws ec2 get-transit-gateway-route-table-propagations --transit-gateway-route-table-id tgw-rtb-012345

# Run automated network reachability analyzer
aws ec2 start-network-insights-access-scope-analysis --network-insights-access-scope-id nias-01234

# Verify VPC subnet routes point to TGW
aws ec2 describe-route-tables --filters 'Name=vpc-id,Values=vpc-prod-a' --query 'RouteTables[*].Routes'

```

### 🗣️ Ideal Interview Candidate Answer (Verbal Pitch)
> "When Transit Gateway routing breaks, I immediately search the TGW route table for 'blackhole' state routes, which occur when attachments are modified or deleted. Then I check both sides of the two-way relationship: TGW Associations determine where incoming packets lookup routes, while TGW Propagations advertise routes. Finally, I use AWS Network Reachability Analyzer to validate end-to-end hop validation without sending live packets."

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← AWS Cloud Infrastructure & Solutions Scenarios: Architecture & Scaling Scenarios](02-Architecture-and-Scaling-Scenarios.md) | [Index](../../../README.md) | [AWS Cloud Infrastructure & Solutions Scenarios: Rapid-Fire Drills & Master Cheat Sheet →](04-Rapid-Fire-Interview-Drills-and-Cheat-Sheet.md) |

