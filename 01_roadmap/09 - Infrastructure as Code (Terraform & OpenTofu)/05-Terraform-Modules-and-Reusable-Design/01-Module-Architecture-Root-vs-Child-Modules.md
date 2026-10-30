# 01 - Module Architecture: Root vs Child Modules

## 1. What Is a Terraform Module?

In software engineering, functions and classes allow developers to encapsulate complex logic, hide internal implementation details, and expose clean, reusable interfaces. 

In Infrastructure as Code, a **Terraform Module** fulfills the exact same architectural purpose. A module is a self-contained container for multiple resources that are used together. Instead of repeatedly writing 60 lines of raw HCL code every time an application requires an Amazon VPC (subnets, route tables, internet gateways, NAT gateways, and flow logs), an engineering team packages that architecture into a reusable `vpc` module. Any team can then instantiate a production-ready VPC by calling the module with just 3 or 4 input variables.

```text
+---------------------------------------------------------------------------------------------------+
|                        THE MODULE ABSTRACTION & ENCAPSULATION PATTERN                             |
+---------------------------------------------------------------------------------------------------+

 CALLER / ROOT MODULE (Simple Interface)
 ┌────────────────────────────────────────────────────────┐
 │ module "production_vpc" {                              │
 │   source   = "git::https://github.com/org/tf-vpc.git"  │
 │   vpc_cidr = "10.100.0.0/16"                           │
 │   az_count = 3                                         │
 │ }                                                      │
 └───────────────────────────┬────────────────────────────┘
                             │ Inputs passed through public interface
                             ▼
 CHILD MODULE IMPLEMENTATION (Encapsulated Black Box)
 ┌──────────────────────────────────────────────────────────────────────────────────┐
 │ Hidden Internal Resources Created Automatically:                                 │
 │  ├── aws_vpc.main (10.100.0.0/16)                                                │
 │  ├── aws_subnet.public[0, 1, 2] & aws_subnet.private[0, 1, 2]                     │
 │  ├── aws_internet_gateway.gw                                                     │
 │  ├── aws_nat_gateway.nat[0, 1, 2] (High Availability Multi-AZ)                   │
 │  └── aws_route_table.public & aws_route_table.private                            │
 └───────────────────────────┬──────────────────────────────────────────────────────┘
                             │ Outputs exported back to Root Module
                             ▼
 RETURNED OUTPUTS:
 ┌────────────────────────────────────────────────────────┐
 │ module.production_vpc.vpc_id                           │
 │ module.production_vpc.private_subnet_ids               │
 └────────────────────────────────────────────────────────┘
```

---

## 2. Root Module vs. Child Module: Scope and Lifecycle

Every Terraform configuration has at least one module, known as the **Root Module**:

### 1. The Root Module (The Application Orchestrator)
- The working directory containing the `.tf` files where an engineer or CI/CD runner executes `terraform init`, `terraform plan`, and `terraform apply`.
- The Root Module is responsible for configuring the **remote backend** (`backend "s3" {}`), provider credentials, and wiring together various child modules.

### 2. The Child Module (The Reusable Component)
- Any module that is called from another module using a `module "<NAME>" {}` block.
- A Child Module can be stored locally in a subfolder (`source = "./modules/database"`) or sourced remotely from a Git repository, Amazon S3 bucket, or private Terraform Registry.
- A Child Module can itself call other child modules, creating a module hierarchy.

---

## 3. The Strict Encapsulation Principle (The "Black Box" Rule)

A foundational architectural constraint in Terraform is that **modules enforce strict scope isolation**:

1. **Child Modules Cannot See Caller Variables:** A Child Module has zero access to variables declared in the Root Module. Every piece of information the child module needs must be explicitly passed as an argument in the `module` call block.
2. **Root Modules Cannot Access Internal Child Resources:** If a Child Module creates a resource named `aws_security_group.db_sg`, the Root Module cannot read `aws_security_group.db_sg.id` directly. The Child Module must explicitly export that value in its **`outputs.tf`** file. If it is not exported, it remains completely hidden from the outside world.

```text
+---------------------------------------------------------------------------------------------------+
|                        THE SCOPE ISOLATION FIREWALL (BLACK BOX MODEL)                             |
+---------------------------------------------------------------------------------------------------+

 ROOT MODULE (Calling Context)                CHILD MODULE (Isolated Context)
 ┌───────────────────────────────┐            ┌───────────────────────────────┐
 │ var.environment = "prod"      │──X NO! X──►│ (Cannot see var.environment   │
 │                               │            │  unless passed as an input!)  │
 │                               │            │                               │
 │ (Cannot see internal          │◄──X NO! X──│ resource "aws_vpc" "internal" │
 │  aws_vpc.internal.id          │            │                               │
 │  unless exported in outputs!) │            │ output "vpc_id" {             │
 │                               │◄──YES! ────│   value = aws_vpc.internal.id │
 │ Reads: module.vpc.vpc_id      │            │ }                             │
 └───────────────────────────────┘            └───────────────────────────────┘
```

---

## 4. Standard Enterprise Module Directory Layout

To maintain consistency across large organizations, production modules strictly follow the standard three-file directory layout:

```text
modules/aws-microservice/
├── main.tf           # Contains the actual resources and data sources
├── variables.tf      # Declares all input variables with types, descriptions, & validation
├── outputs.tf        # Declares all exported output values for callers
├── versions.tf       # Declares required terraform and provider version constraints
└── README.md         # Documentation generated automatically via terraform-docs
```

### 1. `variables.tf` (Explicit Input Contract)
```hcl
variable "cluster_name" {
  type        = string
  description = "The name of the Kubernetes EKS cluster."
  
  validation {
    condition     = length(var.cluster_name) <= 40 && can(regex("^[a-zA-Z0-9-]+$", var.cluster_name))
    error_message = "Cluster name must be alphanumeric with hyphens, maximum 40 characters."
  }
}

variable "node_count" {
  type        = number
  description = "Desired number of worker nodes."
  default     = 3
}
```

### 2. `outputs.tf` (Exported Contract)
```hcl
output "cluster_endpoint" {
  description = "The API server HTTPS endpoint for the EKS cluster."
  value       = aws_eks_cluster.main.endpoint
}

output "cluster_certificate_authority" {
  description = "Base64-encoded certificate data required to communicate with the cluster."
  value       = aws_eks_cluster.main.certificate_authority[0].data
  sensitive   = true # Protects sensitive data in terminal output
}
```

---

## 5. Multi-Region and Multi-Account: Explicit Provider Injection

By default, child modules inherit default provider configurations from the root module. However, when deploying multi-region infrastructure (e.g., creating a primary S3 bucket in `us-east-1` and a replication bucket in `eu-west-1`), the root module must explicitly pass provider aliases using the **`providers`** map:

```hcl
# In Root Module:
provider "aws" {
  alias  = "primary"
  region = "us-east-1"
}

provider "aws" {
  alias  = "replica"
  region = "eu-west-1"
}

module "disaster_recovery_storage" {
  source = "./modules/replicated-storage"

  # Explicit provider mapping into child module:
  providers = {
    aws.primary_region = aws.primary
    aws.backup_region  = aws.replica
  }
}
```

---

## 6. SRE Module Anti-Patterns to Avoid

### 1. The "Russian Doll" Anti-Pattern (Excessive Nesting)
Nesting modules inside modules inside modules (more than 2 levels deep) creates severe maintenance bottlenecks:
- Passing variables down 5 levels of nesting requires duplicating variable declarations in every intermediate layer.
- Debugging execution plan errors becomes nearly impossible.
- **Rule of Thumb:** Keep modules flat. Build composable building blocks that are assembled in the root module, rather than monolithic nested wrappers.

### 2. Unpinned Remote Sources
Never reference remote modules using floating branches (e.g., `?ref=main` or `?ref=master`). If another engineer pushes a breaking change to `main`, your next `terraform init` will pull untested code into production. Always pin to an immutable Git tag or semantic release:
```hcl
source = "git::https://github.com/enterprise/tf-modules.git//vpc?ref=v2.4.1"
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← Prev Module (04-Terraform-State-Management-and-Remote-Backend)](../04-Terraform-State-Management-and-Remote-Backend/12-Quick-Revision.md) | [Index](../../../README.md) | [02 - Module Inputs Outputs and Composition →](./02-Module-Inputs-Outputs-and-Composition.md) |
