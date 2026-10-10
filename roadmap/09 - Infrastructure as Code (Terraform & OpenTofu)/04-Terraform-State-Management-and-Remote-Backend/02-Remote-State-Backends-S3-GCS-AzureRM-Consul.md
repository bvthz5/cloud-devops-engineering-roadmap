# 02 - Remote State Backends: S3, GCS, AzureRM & Consul

## 1. Why Local State Fails in Team Environments

When an engineer runs Terraform on their local workstation without a backend configured, Terraform creates a local file named `terraform.tfstate`. 

While acceptable for local experimentation, storing state locally is catastrophic in team and enterprise production environments:
1. **The Concurrency Collision Disaster:** If Engineer Alice runs `terraform apply` to modify an AWS Security Group while Engineer Bob simultaneously runs `terraform apply` to resize an EC2 instance, neither engineer is aware of the other's operation. The two commands issue competing cloud API requests, overwrite each other's local state, and leave the real-world infrastructure in a partially recorded, corrupted limbo.
2. **Loss of State Equals Loss of Infrastructure:** If an engineer's laptop hard drive fails, is lost, or is wiped, the state file is permanently destroyed. The cloud resources continue running in AWS, but Terraform has completely forgotten them. Any future attempt to run `terraform apply` will attempt to recreate existing resources, failing with duplicate resource name errors.
3. **Severe Security and Credential Leaks:** Storing state on unmanaged developer laptops spreads unencrypted database passwords and cloud API keys across dozens of personal endpoints.

A **Remote State Backend** solves these vulnerabilities by centralizing state in a durable, highly available cloud object store with automated concurrency locking, encryption at rest, and versioned backups.

```text
+---------------------------------------------------------------------------------------------------+
|                        LOCAL STATE DISASTER VS CENTRALIZED REMOTE BACKEND                         |
+---------------------------------------------------------------------------------------------------+

 LOCAL STATE COLLISION (CORRUPTION & CONFLICT):
 ┌──────────────────────┐
 │ Engineer Alice       │──────► Writes local state A ──► Corrupted
 └──────────────────────┘                                 Production
 ┌──────────────────────┐                                 Infrastructure!
 │ Engineer Bob         │──────► Writes local state B ──►
 └──────────────────────┘

 PRODUCTION REMOTE BACKEND (AWS S3 + DYNAMODB LOCKING):
 ┌──────────────────────┐        1. Request Exclusive Lock
 │ Engineer Alice       │───────────────────────────────────────┐
 └──────────────────────┘                                       │
                                                                ▼
 ┌──────────────────────┐        2. Lock Denied! (Waits)   ┌────────────────────────┐
 │ Engineer Bob         │─────────────────────────────────►│ AWS DynamoDB Lock Table│
 └──────────────────────┘                                  │ LockID: prod.tfstate   │
                                                           │ Holder: Alice (PID 42) │
                                                           └───────────┬────────────┘
                                                                       │ 3. Writes State Atomically
                                                                       ▼
                                                           ┌────────────────────────┐
                                                           │ Amazon S3 Bucket       │
                                                           │ - Encrypted with KMS   │
                                                           │ - Versioning Enabled   │
                                                           └────────────────────────┘
```

---

## 2. Production Pattern 1: AWS S3 + DynamoDB (The Enterprise Standard)

The combination of Amazon S3 for durable object storage and Amazon DynamoDB for distributed locking is the gold standard for AWS-centric organizations.

### Complete S3 Backend Configuration:
```hcl
# File: backend.tf
terraform {
  backend "s3" {
    bucket         = "enterprise-production-terraform-state-us-east-1"
    key            = "core-platform/networking/terraform.tfstate"
    region         = "us-east-1"
    
    # 1. Enforce Server-Side Encryption with Customer-Managed KMS Key
    encrypt        = true
    kms_key_id     = "arn:aws:kms:us-east-1:123456789012:key/a1b2c3d4-e5f6-7890"

    # 2. Concurrency Locking via DynamoDB (Must have primary key "LockID")
    dynamodb_table = "terraform-production-locks"

    # 3. Security: Assume an isolated, dedicated IAM Role for state operations
    role_arn       = "arn:aws:iam::123456789012:role/TerraformStateExecutionRole"
    session_name   = "TerraformDeploymentSession"
  }
}
```

### Essential S3 Bucket Security Checklist:
To protect production state files, the S3 bucket must be provisioned with four mandatory security controls:
1. **Enable S3 Object Versioning:** Every state write creates an immutable historical version. If a corrupt state is pushed, an administrator can instantly roll back to the previous version with zero data loss.
2. **Block All Public Access:** Enable all four S3 Block Public Access flags to prevent accidental public disclosure.
3. **Enforce SSE-KMS Encryption:** Prevent unauthorized reads by enforcing KMS encryption with a strict KMS key policy.
4. **Enable S3 Bucket Replication:** Replicate state objects across multiple AWS regions to guarantee disaster recovery if an entire cloud region experiences an outage.

---

## 3. Production Pattern 2: Google Cloud Storage (GCS)

Google Cloud Storage provides native, built-in object locking without requiring a separate database table:

```hcl
terraform {
  backend "gcs" {
    bucket                      = "enterprise-gcp-tfstate-prod"
    prefix                      = "infrastructure/compute"
    # Native locking supported automatically via GCS Object Generation preconditions!
    kms_encryption_key          = "projects/my-prod/locations/us/keyRings/iac-ring/cryptoKeys/state-key"
    impersonate_service_account = "terraform-deployer@my-prod.iam.gserviceaccount.com"
  }
}
```

---

## 4. Production Pattern 3: Azure Blob Storage (`azurerm`)

Azure Blob Storage leverages native Blob lease locking and OIDC Workload Identity Federation:

```hcl
terraform {
  backend "azurerm" {
    resource_group_name  = "rg-terraform-mgmt"
    storage_account_name = "sacorptfstateprod01"
    container_name       = "tfstate"
    key                  = "production/networking.tfstate"
    # Passwordless authentication via Azure AD / Entra ID Workload Identity:
    use_oidc             = true
    use_azuread_auth     = true
  }
}
```

---

## 5. Cross-State Architecture: Decoupling with `terraform_remote_state`

In mature enterprise architectures, you **never** manage an entire company's infrastructure in a single state file. You decouple the infrastructure into independent layers:
- Layer 1: Core Networking (VPC, Subnets, Gateways)
- Layer 2: Shared Datastores (RDS, Redis, Elasticsearch)
- Layer 3: Application Workloads (EKS, ECS, Lambda)

When the Application team needs to know the VPC ID created by the Networking team, they query the remote state output using the **`terraform_remote_state`** data source:

```hcl
# In the Application Team's main.tf:
data "terraform_remote_state" "networking" {
  backend = "s3"
  config = {
    bucket = "enterprise-production-terraform-state-us-east-1"
    key    = "core-platform/networking/terraform.tfstate"
    region = "us-east-1"
  }
}

# Reference outputs cleanly:
resource "aws_instance" "web" {
  ami           = "ami-0c55b159cbfafe1f0"
  instance_type = "t3.medium"
  # Consumes the subnet ID exported by the networking team:
  subnet_id     = data.terraform_remote_state.networking.outputs.public_subnet_ids[0]
}
```

---

## 6. Migration Playbook: Moving from Local to Remote State

Migrating an existing local state file to a centralized cloud backend is fully automated:

```bash
# 1. Add the "backend" configuration block to your main.tf / backend.tf

# 2. Run terraform init with the migration flag:
terraform init -migrate-state

# Output:
# Terraform will detect that you have an existing local state file and prompt:
# "Do you want to copy existing state to the new backend?"
# Type "yes"

# 3. Verify the state was copied by listing remote resources:
terraform state list

# 4. Safely delete the old local terraform.tfstate file from your machine:
rm terraform.tfstate*
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 01 - Terraform State Purpose Structure and Internals](./01-Terraform-State-Purpose-Structure-and-Internals.md) | [Index](../../../README.md) | [03 - State Locking Concurrency and Force Unlock →](./03-State-Locking-Concurrency-and-Force-Unlock.md) |
