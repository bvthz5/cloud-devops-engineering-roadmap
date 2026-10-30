# 02 - DRY Backend & Provider Configuration

## 1. The Configuration Sprawl Problem in Native Terraform

In native Terraform, every directory that represents an independent state boundary must contain an explicit `terraform { backend "s3" {} }` block and an explicit `provider "aws" {}` block.

In an enterprise cloud architecture with 50 microservices deployed across Development, Staging, and Production (150 total state files):
1. **150 Duplicate Backend Definitions:** The exact same S3 bucket, DynamoDB lock table, region, and encryption settings are copy-pasted 150 times.
2. **High Risk of State Key Collisions:** A developer copy-pasting a backend block from `dev/vpc` into `dev/database` might forget to update the `key = "dev/vpc/terraform.tfstate"` attribute. When they run `terraform apply`, **the database state overwrites the VPC state**, corrupting the entire network tracking data!
3. **Provider Configuration Drift:** If an organization mandates that all AWS resources must include an organizational billing tag (`default_tags { tags = { ManagedBy = "IaC" } }`), an engineer must manually edit all 150 provider blocks.

Terragrunt completely eliminates this configuration sprawl through its **`remote_state`** and **`generate`** engines.

```text
+---------------------------------------------------------------------------------------------------+
|                        CENTRALIZED DRY BACKEND SYNTHESIS ARCHITECTURE                             |
+---------------------------------------------------------------------------------------------------+

 ROOT TERRAGRUNT CONFIGURATION (/terragrunt.hcl - DEFINED ONCE FOR ENTIRE COMPANY)
 ┌──────────────────────────────────────────────────────────────────────────────────┐
 │ remote_state {                                                                   │
 │   backend = "s3"                                                                 │
 │   config = {                                                                     │
 │     bucket = "enterprise-tfstate"                                                │
 │     key    = "${path_relative_to_include()}/terraform.tfstate"                       │
 │   }                                                                              │
 │ }                                                                                │
 └────────────────────────────────────────┬─────────────────────────────────────────┘
                                          │ Inherited dynamically by all child folders
         ┌────────────────────────────────┼────────────────────────────────┐
         ▼                                ▼                                ▼
 /live/dev/vpc/                   /live/dev/database/              /live/prod/eks/
 Auto-Synthesized State Key:      Auto-Synthesized State Key:      Auto-Synthesized State Key:
 "live/dev/vpc/terraform.tfstate" "live/dev/database/terraform..." "live/prod/eks/terraform..."
 
 Result: 100% DRY! Impossible to have state key collisions; zero copy-pasting!
```

---

## 2. Deep Dive: The `remote_state` Engine and Automated S3 Bootstrapping

In native Terraform, the Amazon S3 bucket and DynamoDB table used for remote state must be manually pre-created before `terraform init` can succeed.

Terragrunt's **`remote_state`** block automates this entire bootstrapping process:
1. When you run `terragrunt init`, Terragrunt queries AWS to check if the state bucket and DynamoDB table exist.
2. **Automated S3 Provisioning:** If the S3 bucket does not exist, Terragrunt automatically provisions it via AWS SDK calls, immediately configuring:
   - S3 Bucket Versioning (Enabled)
   - S3 Server-Side Encryption (AES256 or AWS KMS)
   - S3 Public Access Block (All 4 public flags set to true)
   - S3 Bucket Access Logging
3. **Automated DynamoDB Provisioning:** If the DynamoDB lock table is missing, Terragrunt automatically creates it with the correct `LockID` primary key.

### Production Root `terragrunt.hcl` Configuration:
```hcl
# File: /infrastructure-live/terragrunt.hcl

# 1. CENTRALIZED REMOTE STATE GENERATION
remote_state {
  backend = "s3"
  generate = {
    path      = "backend.tf"
    if_exists = "overwrite_terragrunt"
  }
  config = {
    bucket         = "corp-terraform-state-${get_aws_account_id()}"
    key            = "${path_relative_to_include()}/terraform.tfstate"
    region         = "us-east-1"
    encrypt        = true
    dynamodb_table = "corp-terraform-state-locks"

    # Enforce least-privilege IAM Role Assumption for state access
    role_arn       = "arn:aws:iam::${get_aws_account_id()}:role/TerraformStateAdminRole"
  }
}

# 2. CENTRALIZED PROVIDER GENERATION WITH MANDATORY CORPORATE TAGS
generate "provider" {
  path      = "provider.tf"
  if_exists = "overwrite_terragrunt"
  contents  = <<-EOF
    provider "aws" {
      region = "us-east-1"
      
      # Mandatory organization-wide tags applied to every cloud resource:
      default_tags {
        tags = {
          ManagedBy   = "Terragrunt"
          Environment = "${path_relative_to_include()}"
          Repository  = "https://github.com/enterprise/infrastructure-live"
        }
      }
    }
  EOF
}
```

---

## 3. The `path_relative_to_include()` Built-in Function

The secret to Terragrunt's collision-free state management is the built-in function **`path_relative_to_include()`**:
- It computes the relative filesystem path from the parent `terragrunt.hcl` file down to the active child directory where `terragrunt` was executed.
- If you execute from `/live/prod/us-east-1/networking/vpc/`, the function returns:
  `live/prod/us-east-1/networking/vpc`
- This string is dynamically interpolated into the S3 `key`, guaranteeing that every single microservice across the enterprise automatically receives a unique, perfectly structured state path.

---

## 4. The Child Module Configuration: Elegantly DRY

Because all backend plumbing, S3 creation, encryption, and provider generation are handled by the root configuration, the child module configuration contains **zero boilerplate**:

```hcl
# File: /infrastructure-live/live/prod/us-east-1/networking/vpc/terragrunt.hcl

# 1. Inherit all backend and provider generation from the root
include "root" {
  path = find_in_parent_folders()
}

# 2. Point to the reusable Terraform module
terraform {
  source = "git::https://github.com/enterprise/tf-modules.git//vpc?ref=v3.2.0"
}

# 3. Supply only the specific variables needed for this deployment
inputs = {
  vpc_name   = "production-vpc"
  cidr_block = "10.200.0.0/16"
  az_count   = 3
}
```

Notice what is missing:
- No `terraform {}` block.
- No `backend "s3" {}` block.
- No `provider "aws" {}` block.
- No hardcoded state keys or bucket names.

---

## 5. Execution Flow: The `.terragrunt-cache` Staging Directory

When an engineer runs `terragrunt apply` in a child folder:
1. Terragrunt creates an ephemeral working directory: `.terragrunt-cache/<hash>/`.
2. It clones the remote module source declared in `terraform.source`.
3. It generates the synthetic `backend.tf` and `provider.tf` files directly inside the cache directory.
4. It sets environment variables matching `inputs = {}` (`TF_VAR_vpc_name="production-vpc"`).
5. It invokes `terraform apply` directly inside the `.terragrunt-cache/` working directory.

This mechanism keeps your Git repository completely free of transient generated files while maintaining 100% compatibility with standard Terraform.

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 01 - Terragrunt Architecture and Purpose](./01-Terragrunt-Architecture-and-Purpose.md) | [Index](../../../README.md) | [03 - Dependency Management and Orchestration →](./03-Dependency-Management-and-Orchestration.md) |
