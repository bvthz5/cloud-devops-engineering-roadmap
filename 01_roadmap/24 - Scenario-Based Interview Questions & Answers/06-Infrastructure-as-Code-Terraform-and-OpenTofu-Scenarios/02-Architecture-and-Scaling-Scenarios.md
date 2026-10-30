# Infrastructure as Code (Terraform & OpenTofu) Interview Scenarios: Architecture & Scaling Scenarios

> **Interview Focus:** Real-world incident response, root-cause analysis (RCA), diagnostic toolchains, and articulate candidate verbal pitches.

---

## 📌 Scenario 4: Terraform Refresh Taking 45 Minutes on a Monolithic State File

### 🚨 The Production Scenario
A company has a single monolithic `terraform.tfstate` file managing 800 resources (VPCs, EKS, RDS, IAM, S3). Every `terraform plan` takes 45 minutes to refresh.

### 🔬 Root Cause Analysis (RCA) & Deep Mechanics
Terraform refreshes state by synchronously querying every cloud provider API sequentially or in small parallel batches. With 800 resources across multiple regions, AWS API rate limiting and latency cause execution times to explode. A single failure or state lock blocks the entire company's infrastructure deployments.

### 🛠️ Step-by-Step Triage & Troubleshooting Workflow
- Step 1: Temporarily use `-refresh=false` only for emergency targeted fixes (not standard practice).
- Step 2: Decompose the monolithic state file into decoupled, domain-specific state files (e.g. Networking, Compute, Storage, Database).
- Step 3: Use `terraform state mv` to migrate resources into separate state backends.
- Step 4: Connect decoupled states using `terraform_remote_state` data sources or Terragrunt dependency blocks.
- Step 5: Measure `terraform plan` time reduction from 45 minutes to <30 seconds.

### 💻 Exact Diagnostic & Remediation Commands
```bash
# Emergency mitigation: target only specific resource (anti-pattern for regular use)
terraform plan -target=aws_security_group.app_sg

# Count total resources managed by single monolithic state file
terraform state list | wc -l

# Extract networking resources into a dedicated state file
terraform state mv -state-out=../networking/terraform.tfstate aws_vpc.main aws_vpc.main

# Use Terragrunt to manage decoupled state directories and dependencies concurrently
terragrunt run-all plan

# Increase concurrent API thread workers to accelerate refresh speed
terraform plan -parallelism=30

```

### 🗣️ Ideal Interview Candidate Answer (Verbal Pitch)
> "Monolithic state files are an architectural anti-pattern that cause API throttling, blast radius expansion, and pipeline gridlock. I break monolithic states down by lifecycle and domain into separate directories: foundation/networking, compute/EKS, and data/RDS. I migrate resources using `terraform state mv` to keep them intact. Each state file now manages only ~30 resources, dropping plan times from 45 minutes to 20 seconds."

---

## 📌 Scenario 5: Importing Legacy Cloud Infrastructure into Terraform Management

### 🚨 The Production Scenario
An existing production AWS environment (10 VPCs, 50 EC2 instances, RDS) was created manually via the AWS Console. You are tasked with bringing it under Terraform control without causing any downtime.

### 🔬 Root Cause Analysis (RCA) & Deep Mechanics
Terraform cannot manage resources that do not exist in its state file. Manually creating HCL code and running `apply` will fail because the resources already exist in AWS (`EntityAlreadyExists`). The resources must be imported into the state file and matched with exact HCL definitions.

### 🛠️ Step-by-Step Triage & Troubleshooting Workflow
- Step 1: Write corresponding HCL resource blocks matching the configuration of existing resources.
- Step 2: In modern Terraform (v1.5+), use declarative `import` blocks (`import { to = ... id = ... }`).
- Step 3: Run `terraform plan -generate-config-out=generated.tf` to let Terraform automatically generate HCL.
- Step 4: Refactor and clean up the generated HCL code to adhere to module standards.
- Step 5: Run `terraform plan` and verify the diff shows `0 to add, 0 to change, 0 to destroy`.

### 💻 Exact Diagnostic & Remediation Commands
```bash
# Traditional CLI import of existing AWS VPC into Terraform state
terraform import aws_vpc.imported vpc-0123456789abcdef0

# Modern declarative import block definition
cat <<EOF > import.tf
import {
  to = aws_s3_bucket.data_lake
  id = "enterprise-data-lake-prod"
}
EOF

# Generate complete verified HCL resource block automatically from live cloud state
terraform plan -generate-config-out=generated_s3.tf

# Verify zero-diff reconciliation between imported state and generated HCL
terraform plan

# Bind the imported resources permanently to state with zero cloud modification
terraform apply

```

### 🗣️ Ideal Interview Candidate Answer (Verbal Pitch)
> "To import legacy infrastructure without downtime, I use Terraform 1.5+ declarative `import` blocks. I write `import { to = aws_vpc.main id = 'vpc-xxx' }` and run `terraform plan -generate-config-out=vpc.tf`. Terraform queries the cloud API and writes the exact matching HCL code for me. I review and sanitize the generated HCL, run `terraform plan` to confirm zero diff, and apply it. This ensures zero downtime and 100% state accuracy."

---

## 📌 Scenario 6: Sensitive Passwords and Private Keys Exposed in Terraform State Files

### 🚨 The Production Scenario
A security audit finds that database master passwords and private TLS keys generated via `random_password` and `tls_private_key` are stored in plain text inside `terraform.tfstate` in an S3 bucket.

### 🔬 Root Cause Analysis (RCA) & Deep Mechanics
Terraform state files must record the values of all resource attributes to calculate diffs. By design, any sensitive output or resource attribute is stored in unencrypted JSON in the state file. Even if marked `sensitive = true` in variables, that only masks the value from CLI terminal output, NOT from the state file.

### 🛠️ Step-by-Step Triage & Troubleshooting Workflow
- Step 1: Encrypt the remote state storage backend with customer-managed KMS keys (SSE-KMS).
- Step 2: Restrict S3 bucket policy and DynamoDB IAM permissions strictly to the CI/CD pipeline role.
- Step 3: Avoid generating static secrets in Terraform; integrate dynamic secrets via HashiCorp Vault or AWS Secrets Manager.
- Step 4: In modern Terraform (v1.6+), use ephemeral resources or write-only attributes.
- Step 5: Rotate any database credentials or private keys that were historically stored in unencrypted states.

### 💻 Exact Diagnostic & Remediation Commands
```bash
# Enforce strict AWS KMS envelope encryption on Terraform state S3 bucket
aws s3api put-bucket-encryption --bucket my-tf-state --server-side-encryption-configuration '{"Rules": [{"ApplyServerSideEncryptionByDefault": {"SSEAlgorithm": "aws:kms", "KMSMasterKeyId": "arn:aws:kms:..."}}]}'

# Audit state file to identify plaintext passwords stored in attributes
terraform state pull | jq '.resources[].instances[].attributes.password'

# Reference external secret manager instead of generating static passwords in HCL
cat <<EOF > secrets.tf
data "aws_secretsmanager_secret_version" "db_creds" {
  secret_id = "prod/db/credentials"
}
EOF

# Block all public access to state bucket
aws s3api put-public-access-block --bucket my-tf-state --public-access-block-configuration BlockPublicAcls=true,IgnorePublicAcls=true,BlockPublicPolicy=true,RestrictPublicBuckets=true

# Re-initialize backend after applying encryption and policy hardening
terraform init -reconfigure

```

### 🗣️ Ideal Interview Candidate Answer (Verbal Pitch)
> "Marking a variable `sensitive = true` only hides it from console logs; it is still stored in plaintext inside `terraform.tfstate`. To secure state secrets: 1) Enforce SSE-KMS encryption on the S3 bucket with strict IAM bucket policies granting access only to the CI/CD execution role, 2) Enable S3 bucket versioning and Object Lock to prevent tampering, and 3) Decouple secret generation from Terraform by using HashiCorp Vault or AWS Secrets Manager to inject secrets dynamically at runtime."

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← Infrastructure as Code (Terraform & OpenTofu) Interview Scenarios: Production Incidents & Triage Scenarios](01-Production-Incidents-and-Triage-Scenarios.md) | [Index](../../../README.md) | [Infrastructure as Code (Terraform & OpenTofu) Interview Scenarios: Security & Disaster Recovery Scenarios →](03-Security-and-Disaster-Recovery-Scenarios.md) |

