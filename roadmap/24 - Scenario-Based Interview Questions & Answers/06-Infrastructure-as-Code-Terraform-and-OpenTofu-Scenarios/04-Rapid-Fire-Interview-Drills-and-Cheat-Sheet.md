# Infrastructure as Code (Terraform & OpenTofu) Interview Scenarios: Rapid-Fire Drills & Master Cheat Sheet

> **Interview Focus:** Real-world incident response, root-cause analysis (RCA), diagnostic toolchains, and articulate candidate verbal pitches.

---

## 📌 Scenario 10: Dynamic Cost Spikes Caught Pre-Apply in CI/CD via Infracost

### 🚨 The Production Scenario
A developer submits a pull request changing an AWS NAT Gateway architecture and upgrading an RDS database instance from `db.t3.medium` to `db.m5.24xlarge`, which would increase monthly spend by $15,000.

### 🔬 Root Cause Analysis (RCA) & Deep Mechanics
Without shift-left FinOps tooling in the CI/CD pipeline, infrastructure cost changes are invisible until the monthly cloud bill arrives. Engineers might accidentally select oversized instances or provision expensive Multi-AZ read replicas without realizing the financial impact.

### 🛠️ Step-by-Step Triage & Troubleshooting Workflow
- Step 1: Integrate `Infracost` CLI into the GitHub Actions / GitLab CI pull request pipeline.
- Step 2: Generate a JSON cost diff on every PR comparing base branch against the PR's Terraform plan.
- Step 3: Automatically post an Infracost cost breakdown table as a PR comment.
- Step 4: Configure pipeline guardrails to fail the PR or require FinOps/Engineering Manager approval if projected monthly cost exceeds $500.
- Step 5: Review cheaper alternative architectures (e.g. Serverless Aurora or Graviton `db.r6g.xlarge`) before approval.

### 💻 Exact Diagnostic & Remediation Commands
```bash
# Calculate real-time monthly cost breakdown for current Terraform configuration
infracost breakdown --path .

# Compute exact dollar difference between main branch and proposed pull request changes
infracost diff --path . --compare-to infracost-base.json

# Post formatted cost breakdown directly onto GitHub pull request
infracost comment github --path infracost.json --repo myorg/infra --pull-request 42 --github-token $GITHUB_TOKEN

# Export plan to JSON for automated policy evaluation
terraform plan -out=tfplan.binary && terraform show -json tfplan.binary > tfplan.json

# Enforce Open Policy Agent budget guardrails against Terraform plan
opa eval --data policy.rego --input tfplan.json 'data.terraform.deny'

```

### 🗣️ Ideal Interview Candidate Answer (Verbal Pitch)
> "Catching cost spikes after deployment is a failure of governance; FinOps must shift left into the pull request workflow. I integrate Infracost into our CI/CD pipelines to parse the Terraform plan JSON on every PR. If an engineer upgrades an RDS instance or provisions expensive cross-region endpoints, Infracost posts the exact dollar impact onto the PR and automatically blocks merging if the monthly delta exceeds $500 without FinOps approval."

---

## ⚡ Rapid-Fire Diagnostic Cheat Sheet

| Symptom / Scenario | First Command to Run | Root Cause Hypothesis |
| :--- | :--- | :--- |
| **Scenario 1: Terraform State Lock Stuck During Interrupted CI/CD Run** | `terraform plan` | When Terraform executes, it acquires an exclusive distributed mutex lock on the ... |
| **Scenario 2: Massive Infrastructure Drift Detected during Scheduled Terraform Plan** | `terraform plan -detailed-exitcode` | Infrastructure drift occurs when cloud resources are modified out-of-band direct... |
| **Scenario 3: Accidental Destruction Plan on a Production Database** | `terraform state list \| grep db_instance` | Terraform identifies resources by their address (e.g. `aws_db_instance.main`). R... |
| **Scenario 4: Terraform Refresh Taking 45 Minutes on a Monolithic State File** | `terraform plan -target=aws_security_group.app_sg` | Terraform refreshes state by synchronously querying every cloud provider API seq... |
| **Scenario 5: Importing Legacy Cloud Infrastructure into Terraform Management** | `terraform import aws_vpc.imported vpc-0123456789abcdef0` | Terraform cannot manage resources that do not exist in its state file. Manually ... |
| **Scenario 6: Sensitive Passwords and Private Keys Exposed in Terraform State Files** | `aws s3api put-bucket-encryption --bucket my-tf-state --server-side-encryption-configuration '{"Rules": [{"ApplyServerSideEncryptionByDefault": {"SSEAlgorithm": "aws:kms", "KMSMasterKeyId": "arn:aws:kms:..."}}]}'` | Terraform state files must record the values of all resource attributes to calcu... |
| **Scenario 7: Cloud Provider API Rate Limiting during Large Terraform Deployments** | `terraform apply -parallelism=5` | By default, Terraform executes up to 10 concurrent threads (`-parallelism=10`). ... |
| **Scenario 8: Terraform Dependency Lock File Conflicts Across Engineering Teams** | `terraform providers lock -platform=linux_amd64 -platform=linux_arm64 -platform=darwin_arm64 -platform=windows_amd64` | The `.terraform.lock.hcl` file locks exact provider versions and cryptographic p... |
| **Scenario 9: Circular Dependency Deadlock in Terraform HCL** | `terraform graph \| dot -Tpng > graph.png` | When Security Group A specifies an ingress rule referencing Security Group B in ... |
| **Scenario 10: Dynamic Cost Spikes Caught Pre-Apply in CI/CD via Infracost** | `infracost breakdown --path .` | Without shift-left FinOps tooling in the CI/CD pipeline, infrastructure cost cha... |

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← Infrastructure as Code (Terraform & OpenTofu) Interview Scenarios: Security & Disaster Recovery Scenarios](03-Security-and-Disaster-Recovery-Scenarios.md) | [Index](../../../README.md) | [Configuration Management (Ansible) Interview Scenarios: Production Incidents & Triage Scenarios →](../07-Configuration-Management-Ansible-Scenarios/01-Production-Incidents-and-Triage-Scenarios.md) |

