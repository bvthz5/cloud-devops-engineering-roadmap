# 03 - Module Sources: Registry, Git, S3 & Local

## 1. Source Types

```hcl
# Local path
module "vpc" {
  source = "./modules/vpc"
}

# Terraform Registry
module "vpc" {
  source  = "terraform-aws-modules/vpc/aws"
  version = "~> 5.0"
}

# GitHub (HTTPS)
module "vpc" {
  source = "github.com/myorg/terraform-aws-vpc?ref=v2.1.0"
}

# Generic Git (SSH)
module "vpc" {
  source = "git@github.com:myorg/terraform-aws-vpc.git?ref=v2.1.0"
}

# S3 bucket
module "vpc" {
  source = "s3::https://my-bucket.s3.amazonaws.com/modules/vpc.zip"
}

# GCS bucket
module "vpc" {
  source = "gcs::https://www.googleapis.com/storage/v1/modules/vpc.zip"
}
```

## 2. Version Pinning

```hcl
module "vpc" {
  source  = "terraform-aws-modules/vpc/aws"
  version = "5.4.0"          # Exact
  # version = "~> 5.4"       # >= 5.4.0, < 5.5.0
  # version = ">= 5.0, < 6.0"  # Range
}
```

**Best Practice:** Always pin module versions in production. Use `~>` for minor version flexibility during development.

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [02 - Module Inputs & Outputs](./02-Module-Inputs-Outputs-and-Composition.md) | [README](./README.md) | [04 - Design Patterns](./04-Module-Design-Patterns-and-Best-Practices.md) |
