# 02 - Module Inputs, Outputs & Composition

## 1. Module Input Variables

```hcl
# modules/vpc/variables.tf
variable "vpc_cidr" {
  description = "CIDR block for the VPC"
  type        = string
  validation {
    condition     = can(cidrhost(var.vpc_cidr, 0))
    error_message = "Must be a valid CIDR block."
  }
}

variable "environment" {
  description = "Deployment environment (dev/staging/prod)"
  type        = string
}
```

## 2. Module Outputs

```hcl
# modules/vpc/outputs.tf
output "vpc_id" {
  description = "ID of the created VPC"
  value       = aws_vpc.main.id
}

output "private_subnet_ids" {
  description = "List of private subnet IDs"
  value       = aws_subnet.private[*].id
}
```

## 3. Module Composition Pattern

```hcl
# Root module composes multiple child modules
module "networking" {
  source      = "./modules/vpc"
  vpc_cidr    = "10.0.0.0/16"
  environment = var.environment
}

module "database" {
  source     = "./modules/rds"
  subnet_ids = module.networking.private_subnet_ids  # ← Composed
  vpc_id     = module.networking.vpc_id              # ← Composed
}

module "compute" {
  source     = "./modules/ecs"
  subnet_ids = module.networking.private_subnet_ids
  db_endpoint = module.database.endpoint             # ← Composed
}
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [01 - Module Architecture](./01-Module-Architecture-Root-vs-Child-Modules.md) | [README](./README.md) | [03 - Module Sources](./03-Module-Sources-Registry-Git-S3-and-Local.md) |
