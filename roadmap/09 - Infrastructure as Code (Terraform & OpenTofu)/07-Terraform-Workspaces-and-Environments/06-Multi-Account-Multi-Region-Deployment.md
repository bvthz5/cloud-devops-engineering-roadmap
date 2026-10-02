# 06 - Multi-Account & Multi-Region Deployment

## 1. Provider Aliases for Multi-Region

```hcl
provider "aws" {
  region = "us-east-1"
  alias  = "us_east"
}

provider "aws" {
  region = "eu-west-1"
  alias  = "eu_west"
}

# Deploy VPC in both regions
module "vpc_us" {
  source = "./modules/vpc"
  providers = { aws = aws.us_east }
  cidr   = "10.0.0.0/16"
}

module "vpc_eu" {
  source = "./modules/vpc"
  providers = { aws = aws.eu_west }
  cidr   = "10.1.0.0/16"
}
```

## 2. Cross-Account with assume_role

```hcl
provider "aws" {
  region = "us-east-1"
  alias  = "prod"
  assume_role {
    role_arn     = "arn:aws:iam::PROD_ACCOUNT_ID:role/TerraformRole"
    session_name = "terraform-prod"
  }
}
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 05 - Terraform Cloud Workspaces and VCS Workflows](./05-Terraform-Cloud-Workspaces-and-VCS-Workflows.md) | [Index](../../../README.md) | [07 - Real World Scenarios →](./07-Real-World-Scenarios.md) |
