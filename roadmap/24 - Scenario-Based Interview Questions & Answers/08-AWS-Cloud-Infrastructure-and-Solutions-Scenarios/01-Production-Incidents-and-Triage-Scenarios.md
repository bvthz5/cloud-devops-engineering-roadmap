# AWS Cloud Infrastructure & Solutions Scenarios: Production Incidents & Triage Scenarios

> **Interview Focus:** Real-world incident response, root-cause analysis (RCA), diagnostic toolchains, and articulate candidate verbal pitches.

---

## 📌 Scenario 1: Cross-Account S3 Bucket Access Fails with 403 Forbidden Despite Valid IAM Policy

### 🚨 The Production Scenario
An EC2 instance in Account A running a data processing workload attempts to fetch data from an S3 bucket in Account B using an IAM role. The IAM role in Account A has 's3:GetObject' and 's3:ListBucket' allowed, but all AWS CLI and SDK calls fail with 403 Access Denied.

### 🔬 Root Cause Analysis (RCA) & Deep Mechanics
Cross-account S3 access requires explicit authorization in BOTH Account A's IAM policy AND Account B's S3 Bucket Policy (resource policy), or KMS key policy permissions if SSE-KMS encryption is active. Furthermore, if the objects in Account B were uploaded by a third principal without 'bucket-owner-full-control', the bucket owner may not own the object ACL.

### 🛠️ Step-by-Step Triage & Troubleshooting Workflow
- Confirm the caller identity and assumed role ARN using 'aws sts get-caller-identity'.
- Examine Account B's S3 bucket policy for explicit Principal matching Account A's IAM role ARN.
- Verify if the S3 bucket is encrypted with SSE-KMS; check if KMS Key Policy grants 'kms:Decrypt' to Account A's role.
- Check for explicit SCPs (Service Control Policies) at Account A or B's AWS Organizations root.
- Update S3 Bucket Policy and KMS Key Policy to grant cross-account access.

### 💻 Exact Diagnostic & Remediation Commands
```bash
# Verify IAM principal, role ARN, and account ID
aws sts get-caller-identity

# Inspect S3 bucket policy in Account B
aws s3api get-bucket-policy --bucket data-prod-account-b

# Inspect KMS Key policy for cross-account decrypt rights
aws kms get-key-policy --key-id arn:aws:kms:us-east-1:222222222222:key/xxx --policy-name default

# Test object upload ensuring bucket owner full control ownership
aws s3 cp test.txt s3://data-prod-account-b/test.txt --acl bucket-owner-full-control

```

### 🗣️ Ideal Interview Candidate Answer (Verbal Pitch)
> "For cross-account S3 access, IAM permissions in Account A are only half the handshake. Account B's S3 bucket policy must explicitly permit the external role ARN. If KMS encryption is enabled, Account B's KMS key policy must also allow kms:Decrypt. Finally, S3 Object Ownership should be configured with BucketOwnerEnforced to eliminate legacy ACL ownership pitfalls."

---

## 📌 Scenario 2: EKS Worker Nodes Fail to Join Cluster After Subnet Migration

### 🚨 The Production Scenario
Following a network refactor where worker nodes were migrated to new private subnets, freshly provisioned EKS managed node groups remain in 'NotReady' status and do not appear in 'kubectl get nodes'.

### 🔬 Root Cause Analysis (RCA) & Deep Mechanics
Worker nodes require route table connectivity to the EKS control plane API endpoint (via private VPC endpoint or NAT Gateway), proper VPC DNS resolution/hostnames enabled, and the aws-auth ConfigMap (or EKS Access Entries) must authorize the node IAM role. If private cluster endpoint is enabled without proper security group egress on 443 to the cluster security group, kubelet cannot handshake.

### 🛠️ Step-by-Step Triage & Troubleshooting Workflow
- Check node EC2 instance system logs via AWS Console or CLI for kubelet bootstrap failures.
- Inspect subnet route tables for 0.0.0.0/0 route to NAT gateway or VPC Endpoints for ECR, S3, STS, and EKS.
- Validate security group rules on both cluster security group and node group security group on TCP port 443.
- Verify VPC settings: 'enableDnsHostnames' and 'enableDnsSupport' must both be set to true.
- Inspect aws-auth ConfigMap in kube-system or check EKS Access Entries.

### 💻 Exact Diagnostic & Remediation Commands
```bash
# Inspect EC2 instance console bootstrap logs
aws ec2 get-console-output --instance-id i-0abcdef1234567890

# Check subnet route table associations
aws ec2 describe-route-tables --filters 'Name=association.subnet-id,Values=subnet-0123456'

# Verify cluster and node security group rules
aws ec2 describe-security-groups --group-ids sg-ekscluster sg-nodes

# Verify node instance IAM role mapping in aws-auth
kubectl get configmap aws-auth -n kube-system -o yaml

```

### 🗣️ Ideal Interview Candidate Answer (Verbal Pitch)
> "When EKS nodes fail to join, it is virtually always network egress or IAM authentication. I immediately check EC2 console logs for the bootstrap script output. Then I verify the node can resolve and reach the EKS API endpoint via port 443, confirm route tables have NAT or VPC endpoints, ensure VPC DNS hostnames are enabled, and verify the node instance role matches aws-auth or EKS Access Entries."

---

## 📌 Scenario 3: ALB 504 Gateway Timeout Spikes During Traffic Peaks

### 🚨 The Production Scenario
During a flash marketing campaign, users receive HTTP 504 Gateway Timeouts from an Application Load Balancer. CloudWatch metrics show TargetResponseTime spiking above 60 seconds while ALB UnHealthyHostCount remains normal.

### 🔬 Root Cause Analysis (RCA) & Deep Mechanics
HTTP 504 on an ALB indicates the load balancer established a connection to the target EC2/ECS/EKS instances, but the target failed to return a response within the configured idle timeout. Common causes include backend thread pool exhaustion, database connection pool exhaustion, or downstream API locks.

### 🛠️ Step-by-Step Triage & Troubleshooting Workflow
- Check CloudWatch metrics: compare HTTPCode_ELB_504_Count against TargetResponseTime and ActiveConnectionCount.
- Inspect ALB access logs filtering for elb_status_code == 504 to identify specific API paths or backend target IPs.
- Connect to target instances and inspect web server connection pools (e.g., Nginx, Gunicorn, Tomcat) and CPU/memory utilization.
- Check database active queries, lock contention, and connection limits in RDS/Aurora.
- Temporarily increase ALB idle timeout if legitimate long polling, but fix backend connection pooling and horizontal scaling thresholds.

### 💻 Exact Diagnostic & Remediation Commands
```bash
# Get ALB ARN
aws elbv2 describe-load-balancers --names app-prod-alb --query 'LoadBalancers[0].LoadBalancerArn'

# Query TargetResponseTime vs 504 counts
aws cloudwatch get-metric-data --metric-data-queries file://query.json --start-time $(date -u -v-1H +%FT%TZ) --end-time $(date -u +%FT%TZ)

# Inspect target health status and response timings
aws elbv2 describe-target-health --target-group-arn arn:aws:elasticloadbalancing:...:targetgroup/...

# Check open connection backlog on backend instances
ss -s && netstat -ant | grep ESTABLISHED | wc -l

```

### 🗣️ Ideal Interview Candidate Answer (Verbal Pitch)
> "An ALB 504 is an upstream target timeout, not an ALB failure. The ALB waited for the target response until the idle timeout expired. I query ALB access logs to isolate slow endpoints, check RDS connection metrics and lock waits, and inspect the application thread pool saturation. Remediation involves optimizing slow queries, auto-scaling backend targets based on RequestCountPerTarget, and tuning application connection pools."

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← Configuration Management (Ansible) Interview Scenarios: Rapid-Fire Drills & Master Cheat Sheet](../07-Configuration-Management-Ansible-Scenarios/04-Rapid-Fire-Interview-Drills-and-Cheat-Sheet.md) | [Index](../../../README.md) | [AWS Cloud Infrastructure & Solutions Scenarios: Architecture & Scaling Scenarios →](02-Architecture-and-Scaling-Scenarios.md) |

