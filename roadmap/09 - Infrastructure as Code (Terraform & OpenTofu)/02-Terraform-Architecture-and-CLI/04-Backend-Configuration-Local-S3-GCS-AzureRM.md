# 04 - Backend Configuration: Local, S3, GCS, AzureRM

## 1. What Is a Backend?

A backend defines **where Terraform stores its state** and **how operations are executed**. The default is a local `terraform.tfstate` file, which is unsuitable for team collaboration.

## 2. Backend Comparison

| Backend | State Storage | State Locking | Encryption | Use Case |
|---|---|---|---|---|
| **local** | Local filesystem | ❌ No | ❌ No | Solo development only |
| **s3** | AWS S3 bucket | ✅ DynamoDB | ✅ SSE-S3/KMS | AWS teams |
| **gcs** | GCP Cloud Storage | ✅ Built-in | ✅ CMEK | GCP teams |
| **azurerm** | Azure Blob Storage | ✅ Blob lease | ✅ SSE | Azure teams |
| **consul** | HashiCorp Consul | ✅ Built-in | ✅ TLS | Multi-cloud (self-hosted) |
| **cloud** | Terraform Cloud | ✅ Built-in | ✅ Built-in | HCP Terraform users |

## 3. AWS S3 Backend (Most Common)

```hcl
# terraform.tf
terraform {
  backend "s3" {
    bucket         = "mycompany-terraform-state"
    key            = "networking/vpc/terraform.tfstate"
    region         = "us-east-1"
    encrypt        = true
    dynamodb_table = "terraform-locks"      # State locking
    kms_key_id     = "alias/terraform-state" # KMS encryption
  }
}
```

### Bootstrap Script for S3 Backend
```bash
# Create the S3 bucket
aws s3api create-bucket \
  --bucket mycompany-terraform-state \
  --region us-east-1

# Enable versioning (state file history)
aws s3api put-bucket-versioning \
  --bucket mycompany-terraform-state \
  --versioning-configuration Status=Enabled

# Create DynamoDB table for state locking
aws dynamodb create-table \
  --table-name terraform-locks \
  --attribute-definitions AttributeName=LockID,AttributeType=S \
  --key-schema AttributeName=LockID,KeyType=HASH \
  --billing-mode PAY_PER_REQUEST
```

## 4. GCS Backend

```hcl
terraform {
  backend "gcs" {
    bucket = "mycompany-tf-state"
    prefix = "networking/vpc"
  }
}
```

## 5. Azure Blob Backend

```hcl
terraform {
  backend "azurerm" {
    resource_group_name  = "terraform-state-rg"
    storage_account_name = "tfstateaccount"
    container_name       = "tfstate"
    key                  = "networking.terraform.tfstate"
  }
}
```

## 6. Backend Migration

```bash
# Migrate from local to S3
# 1. Add backend "s3" block to terraform.tf
# 2. Run:
terraform init -migrate-state
# Terraform will ask: "Do you want to copy existing state?"  → yes
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [03 - CLI Deep Dive](./03-Terraform-CLI-Deep-Dive-Init-Plan-Apply-Destroy.md) | [README](./README.md) | [05 - DAG & Parallelism](./05-Terraform-Graph-DAG-and-Parallelism.md) |
