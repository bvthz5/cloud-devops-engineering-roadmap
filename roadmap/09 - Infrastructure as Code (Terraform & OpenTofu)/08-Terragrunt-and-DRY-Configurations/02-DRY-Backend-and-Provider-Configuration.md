# 02 - DRY Backend & Provider Configuration

## 1. Root terragrunt.hcl (Backend Generation)

```hcl
# live/terragrunt.hcl
remote_state {
  backend = "s3"
  generate = {
    path      = "backend.tf"
    if_exists = "overwrite_terragrunt"
  }
  config = {
    bucket         = "acme-terraform-state"
    key            = "${path_relative_to_include()}/terraform.tfstate"
    region         = "us-east-1"
    encrypt        = true
    dynamodb_table = "terraform-locks"
  }
}

# Auto-generate provider.tf
generate "provider" {
  path      = "provider.tf"
  if_exists = "overwrite_terragrunt"
  contents  = <<EOF
provider "aws" {
  region = "us-east-1"
}
EOF
}
```

## 2. Module-Level terragrunt.hcl

```hcl
# live/dev/vpc/terragrunt.hcl
include "root" {
  path = find_in_parent_folders()   # Inherits root config
}

terraform {
  source = "../../../modules//vpc"   # Points to shared module
}

inputs = {
  vpc_cidr    = "10.0.0.0/16"
  environment = "dev"
}
```

**Result:** Backend and provider are auto-generated. No duplication across environments.

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [01 - Terragrunt Architecture](./01-Terragrunt-Architecture-and-Purpose.md) | [README](./README.md) | [03 - Dependency Management](./03-Dependency-Management-and-Orchestration.md) |
