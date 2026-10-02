# 03 - Environment Segregation Patterns

## 1. Pattern Overview

```text
PATTERN 1: Per-Account (Strongest Isolation)
  AWS Account: dev-123 -> VPC, EC2, RDS (dev)
  AWS Account: stg-456 -> VPC, EC2, RDS (staging)
  AWS Account: prd-789 -> VPC, EC2, RDS (prod)

PATTERN 2: Per-VPC (Moderate Isolation)
  AWS Account: shared-123
    VPC: dev-vpc   -> EC2, RDS (dev)
    VPC: stg-vpc   -> EC2, RDS (staging)
    VPC: prod-vpc  -> EC2, RDS (prod)

PATTERN 3: Per-Namespace (Weakest Isolation)
  Single K8s Cluster
    Namespace: dev   -> Pods, Services
    Namespace: stg   -> Pods, Services
    Namespace: prod  -> Pods, Services
```

## 2. Enterprise Best Practice: Per-Account + Directory-Based

```hcl
# environments/prod/providers.tf
provider "aws" {
  region = "us-east-1"
  assume_role {
    role_arn = "arn:aws:iam::PROD_ACCOUNT:role/TerraformDeployRole"
  }
}
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [02 - Workspaces vs Directories](./02-Workspaces-vs-Directory-Based-Environments.md) | [README](./README.md) | [04 - Variable Files per Environment](./04-Variable-Files-and-tfvars-per-Environment.md) |
