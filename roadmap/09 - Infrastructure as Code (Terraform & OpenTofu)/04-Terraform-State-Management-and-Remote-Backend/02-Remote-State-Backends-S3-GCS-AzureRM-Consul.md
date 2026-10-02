# 02 - Remote State Backends: S3, GCS, AzureRM & Consul

## 1. Why Remote State?

| Local State | Remote State |
|---|---|
| Single file on disk | Centralized in cloud storage |
| No locking → corruption risk | Automatic locking → safe team collaboration |
| No encryption by default | Encryption at rest + in transit |
| No versioning history | S3 versioning / GCS versions for rollback |
| Cannot share between teams | Cross-team `terraform_remote_state` data source |

## 2. AWS S3 + DynamoDB (Production Standard)

```hcl
terraform {
  backend "s3" {
    bucket         = "acme-corp-terraform-state"
    key            = "services/api/terraform.tfstate"
    region         = "us-east-1"
    encrypt        = true
    kms_key_id     = "arn:aws:kms:us-east-1:123456789:alias/terraform"
    dynamodb_table = "terraform-state-locks"
    
    # Optional: Assume role for cross-account access
    role_arn       = "arn:aws:iam::123456789:role/TerraformStateAccess"
  }
}
```

## 3. GCS Backend

```hcl
terraform {
  backend "gcs" {
    bucket  = "acme-corp-tf-state"
    prefix  = "services/api"
    # Locking is built-in (no separate table needed)
    # Encryption: Google-managed or CMEK
  }
}
```

## 4. Azure Blob Backend

```hcl
terraform {
  backend "azurerm" {
    resource_group_name  = "rg-terraform-state"
    storage_account_name = "acmetfstate"
    container_name       = "tfstate"
    key                  = "services/api.terraform.tfstate"
    use_oidc             = true   # Workload Identity Federation
  }
}
```

## 5. State Sharing with `terraform_remote_state`

```hcl
# In the compute module, read outputs from the networking module
data "terraform_remote_state" "network" {
  backend = "s3"
  config = {
    bucket = "acme-corp-terraform-state"
    key    = "networking/vpc/terraform.tfstate"
    region = "us-east-1"
  }
}

resource "aws_instance" "app" {
  subnet_id = data.terraform_remote_state.network.outputs.private_subnet_ids[0]
}
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [01 - State Purpose](./01-Terraform-State-Purpose-Structure-and-Internals.md) | [README](./README.md) | [03 - State Locking](./03-State-Locking-Concurrency-and-Force-Unlock.md) |
