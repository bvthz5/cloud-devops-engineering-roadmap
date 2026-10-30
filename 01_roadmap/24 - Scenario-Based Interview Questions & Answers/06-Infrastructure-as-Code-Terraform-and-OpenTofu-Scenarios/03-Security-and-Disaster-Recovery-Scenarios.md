# Infrastructure as Code (Terraform & OpenTofu) Interview Scenarios: Security & Disaster Recovery Scenarios

> **Interview Focus:** Real-world incident response, root-cause analysis (RCA), diagnostic toolchains, and articulate candidate verbal pitches.

---

## 📌 Scenario 7: Cloud Provider API Rate Limiting during Large Terraform Deployments

### 🚨 The Production Scenario
During a large infrastructure rollout creating 200 security group rules and subnets, Terraform crashes with `RequestLimitExceeded: Request limit exceeded` from the AWS API.

### 🔬 Root Cause Analysis (RCA) & Deep Mechanics
By default, Terraform executes up to 10 concurrent threads (`-parallelism=10`). When managing complex dependency graphs with hundreds of resources, Terraform floods the cloud provider's API endpoints, exceeding the provider's token bucket rate limits.

### 🛠️ Step-by-Step Triage & Troubleshooting Workflow
- Step 1: Reduce concurrency by running Terraform with `-parallelism=5` or `-parallelism=3`.
- Step 2: Configure the AWS/Azure provider with exponential backoff and retry settings (`max_retries = 10`).
- Step 3: Insert artificial delays using `time_sleep` resources between heavy resource batches.
- Step 4: Request API rate limit increases through cloud provider support for production accounts.
- Step 5: Structure infrastructure into modular deployments applied in sequence.

### 💻 Exact Diagnostic & Remediation Commands
```bash
# Throttle concurrency from default 10 to 5 threads to avoid cloud rate limits
terraform apply -parallelism=5

# Configure provider retry settings to automatically handle transient rate throttling
cat <<EOF > provider.tf
provider "aws" {
  region      = "us-east-1"
  max_retries = 10
}
EOF

# Ultra-conservative rollout speed for strict rate-limited accounts
terraform apply -auto-approve -parallelism=3

# Inspect current cloud API request quota limits
aws service-quotas get-service-quota --service-code ec2 --quota-code L-XXXXX

# Verify that plan completes without hitting API threshold barriers
terraform plan

```

### 🗣️ Ideal Interview Candidate Answer (Verbal Pitch)
> "API rate limiting happens when Terraform's parallel worker threads exceed cloud API token buckets. I mitigate this by: 1) Tuning `-parallelism=5` to reduce API pressure, 2) Configuring `max_retries = 10` inside the cloud provider block so Terraform automatically executes exponential backoff and jitter on 429/RequestLimitExceeded errors, and 3) Using `time_sleep` resources when provisioning components that require asynchronous cloud initialization."

---

## 📌 Scenario 8: Terraform Dependency Lock File Conflicts Across Engineering Teams

### 🚨 The Production Scenario
A CI/CD pipeline fails with `Error: Failed to install provider ... lock file .terraform.lock.hcl does not match configured constraints` after multiple developers upgrade provider plugins.

### 🔬 Root Cause Analysis (RCA) & Deep Mechanics
The `.terraform.lock.hcl` file locks exact provider versions and cryptographic package hashes (including platform-specific hashes for Darwin, Linux, Windows). If a developer on macOS runs `terraform init` without cross-platform checksums, the lock file records only macOS hashes, causing Linux CI runners to fail integrity verification.

### 🛠️ Step-by-Step Triage & Troubleshooting Workflow
- Step 1: Never delete `.terraform.lock.hcl` as a workaround; it is critical for software supply chain security.
- Step 2: Run `terraform providers lock` specifying all target OS platforms (`linux_amd64`, `linux_arm64`, `darwin_arm64`).
- Step 3: Commit the updated `.terraform.lock.hcl` to Git.
- Step 4: Pin provider versions using pessimistic version constraints (`~> 5.30.0`).
- Step 5: Verify clean `terraform init` on Linux CI runners.

### 💻 Exact Diagnostic & Remediation Commands
```bash
# Generate and embed multi-platform cryptographic checksums into provider lock file
terraform providers lock -platform=linux_amd64 -platform=linux_arm64 -platform=darwin_arm64 -platform=windows_amd64

# Inspect locked provider versions and cryptographic h1/zh hashes
cat .terraform.lock.hcl | grep -A 10 'provider'

# Test initialization on target platform
terraform init

# Commit verified multi-platform lock file
git add .terraform.lock.hcl && git commit -m 'chore: update multi-arch provider locks'

# Display tree of all required and installed providers in codebase
terraform providers

```

### 🗣️ Ideal Interview Candidate Answer (Verbal Pitch)
> "Lockfile mismatches occur when a developer initializes Terraform on macOS and commits a lockfile containing only Darwin hashes, breaking Linux CI runners. The proper solution is running `terraform providers lock -platform=linux_amd64 -platform=darwin_arm64`, which fetches and pins cryptographic SHA-256 hashes for all team and CI architectures. This maintains strict supply chain security while preventing pipeline failures."

---

## 📌 Scenario 9: Circular Dependency Deadlock in Terraform HCL

### 🚨 The Production Scenario
A Terraform apply fails with `Error: Cycle: aws_security_group_rule.a, aws_security_group_rule.b` when two microservices need to reference each other's security groups.

### 🔬 Root Cause Analysis (RCA) & Deep Mechanics
When Security Group A specifies an ingress rule referencing Security Group B in its inline block, and Security Group B specifies an ingress rule referencing Security Group A inline, Terraform cannot construct a Directed Acyclic Graph (DAG) and detects a cycle deadlock.

### 🛠️ Step-by-Step Triage & Troubleshooting Workflow
- Step 1: Never define security group rules inline inside `aws_security_group` resource blocks.
- Step 2: Create the `aws_security_group` resources first with zero inline rules.
- Step 3: Use standalone `aws_security_group_rule` resources to define cross-referencing rules.
- Step 4: In modern AWS provider versions, use `aws_vpc_security_group_ingress_rule` to break graph cycles.
- Step 5: Re-run `terraform graph` to confirm the dependency graph is acyclic.

### 💻 Exact Diagnostic & Remediation Commands
```bash
# Generate visual dependency graph to locate circular loop cycles
terraform graph | dot -Tpng > graph.png

# Decouple rule into standalone resource referencing already-created security groups
cat <<EOF > sg_rules.tf
resource "aws_vpc_security_group_ingress_rule" "app_to_db" {
  security_group_id = aws_security_group.db.id
  referenced_security_group_id = aws_security_group.app.id
  ip_protocol = "tcp"
  from_port   = 5432
  to_port     = 5432
}
EOF

# Verify graph cycle error is resolved
terraform plan

# Validate internal HCL syntax and configuration integrity
terraform validate

# Apply decoupled security group architecture safely
terraform apply

```

### 🗣️ Ideal Interview Candidate Answer (Verbal Pitch)
> "Circular dependency cycles happen when security groups reference each other inline, preventing Terraform from determining which to create first. The industry standard fix is never using inline rules. I provision the empty `aws_security_group` shells first, and define ingress/egress rules as standalone `aws_vpc_security_group_ingress_rule` resources. This breaks the dependency cycle, allowing both groups to provision first and rules to attach afterwards."

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← Infrastructure as Code (Terraform & OpenTofu) Interview Scenarios: Architecture & Scaling Scenarios](02-Architecture-and-Scaling-Scenarios.md) | [Index](../../../README.md) | [Infrastructure as Code (Terraform & OpenTofu) Interview Scenarios: Rapid-Fire Drills & Master Cheat Sheet →](04-Rapid-Fire-Interview-Drills-and-Cheat-Sheet.md) |

