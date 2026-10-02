# 01 - Module Architecture: Root vs Child Modules

## 1. Module Hierarchy

```text
┌──────────────────────────────────────────────────────────────┐
│                    ROOT MODULE                                │
│  (The directory where you run terraform apply)               │
│                                                               │
│  main.tf ──► module "vpc" {                                  │
│                source = "./modules/vpc"    ← CHILD MODULE    │
│              }                                                │
│             module "compute" {                                │
│                source = "./modules/compute" ← CHILD MODULE   │
│              }                                                │
└──────────────────────────────────────────────────────────────┘
```

## 2. Module File Structure

```text
modules/
├── vpc/
│   ├── main.tf          # Resource definitions
│   ├── variables.tf     # Input variables
│   ├── outputs.tf       # Output values
│   ├── versions.tf      # Provider version constraints
│   └── README.md        # Module documentation
│
└── compute/
    ├── main.tf
    ├── variables.tf
    ├── outputs.tf
    └── versions.tf
```

## 3. Module Call Lifecycle

```text
terraform init
    │
    ├── 1. Discover module blocks in root module
    ├── 2. Download modules from source (registry/git/local)
    ├── 3. Store in .terraform/modules/
    └── 4. Validate module inputs against variable declarations

terraform plan / apply
    │
    ├── 1. Evaluate root module
    ├── 2. Pass input variables to child modules
    ├── 3. Execute child module resources (respecting DAG)
    └── 4. Collect module outputs back to root
```

## 4. Calling a Module

```hcl
module "vpc" {
  source  = "./modules/vpc"
  
  # Pass input variables
  vpc_cidr    = "10.0.0.0/16"
  environment = var.environment
  
  # Provider configuration inheritance
  providers = {
    aws = aws.us_east
  }
}

# Access module outputs
resource "aws_instance" "app" {
  subnet_id = module.vpc.private_subnet_ids[0]
}
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← Prev Module (04-Terraform-State-Management-and-Remote-Backend)](../04-Terraform-State-Management-and-Remote-Backend/12-Quick-Revision.md) | [Index](../../../README.md) | [02 - Module Inputs Outputs and Composition →](./02-Module-Inputs-Outputs-and-Composition.md) |
