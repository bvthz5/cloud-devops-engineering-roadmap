# 01 - Terragrunt Architecture & Purpose

## 1. What Is Terragrunt and Why Was It Created?

**Terragrunt** is an open-source, thin architectural orchestrator and wrapper for Terraform and OpenTofu (created by Yevgeniy Brikman and the team at Gruntwork). 

Terragrunt was engineered specifically to solve the massive configuration duplication and orchestration limitations that emerge when enterprise organizations scale vanilla Terraform across multiple clouds, regions, environments, and hundreds of microservices.

### The 4 Scalability Bottlenecks of Native Terraform:
1. **The Backend DRY (Don't Repeat Yourself) Nightmare:** In native Terraform, **variables and interpolation are strictly forbidden inside `backend` blocks**. If an organization maintains 60 microservices across 3 environments (`dev`, `stage`, `prod`), engineers are forced to copy-paste the exact same 15-line `backend "s3"` block **180 times**. If an administrator needs to change the KMS key or rename the S3 bucket, they must manually update 180 files.
2. **Provider Duplication Sprawl:** Standard provider configurations, assume-role blocks, and provider aliases must be copy-pasted across every single root directory.
3. **No Multi-Module Orchestration:** Native Terraform cannot execute operations across multiple independent state directories simultaneously. If an infrastructure change touches the `vpc`, `database`, and `kubernetes` directories, an engineer must manually execute `cd vpc && terraform apply`, wait for completion, then `cd ../database && terraform apply`, and finally `cd ../kubernetes && terraform apply`.
4. **Fragile State Inter-Wiring:** Reading outputs from another directory requires writing verbose `data "terraform_remote_state"` blocks with hardcoded bucket names, regions, and state keys.

Terragrunt solves every one of these bottlenecks by acting as an intelligent pre-processor that generates configuration on-the-fly, passes inputs, manages dependencies, and orchestrates multi-module deployments.

```text
+---------------------------------------------------------------------------------------------------+
|                        HOW TERRAGRUNT ORCHESTRATES TERRAFORM UNDER THE HOOD                       |
+---------------------------------------------------------------------------------------------------+

 ┌─────────────────────────────────────────────────────────────────────────────────────────────────┐
 │ USER EXECUTES: "terragrunt run-all apply"                                                        │
 └─────────────────────────────────┬───────────────────────────────────────────────────────────────┘
                                   │
                                   ▼
 ┌─────────────────────────────────────────────────────────────────────────────────────────────────┐
 │ TERRAGRUNT ORCHESTRATOR ENGINE                                                                  │
 │                                                                                                 │
 │  1. Scans all child directories for "terragrunt.hcl"                                            │
 │  2. Evaluates "dependency" blocks ──► Constructs inter-module DAG Dependency Graph               │
 │  3. Evaluates "include" blocks     ──► Inherits root S3 backend & provider configurations       │
 │  4. Dynamically generates in-memory files:                                                      │
 │     - backend.tf (Injects dynamic S3 bucket and unique state key per directory)                 │
 │     - provider.tf (Injects AWS provider with regional variables)                                │
 │  5. Converts terragrunt "inputs = {}" into TF_VAR_* environment variables                       │
 └─────────────────────────────────┬───────────────────────────────────────────────────────────────┘
                                   │
                                   ▼ Executes native binary in topological order
 ┌─────────────────────────────────────────────────────────────────────────────────────────────────┐
 │ TERRAFORM / OPENTOFU CLI                                                                        │
 │                                                                                                 │
 │  Step 1: Runs "terraform apply" inside /vpc                                                     │
 │  Step 2: Collects VPC outputs ──► Injects them into /database and /kubernetes                    │
 │  Step 3: Runs "terraform apply" in parallel inside /database and /kubernetes!                   │
 └─────────────────────────────────────────────────────────────────────────────────────────────────┘
```

---

## 2. Core Terragrunt Architectural Primitives

### 1. `include` (Hierarchical Configuration Inheritance)
Allows child directories to inherit backend configurations, provider blocks, and global variables from a parent `terragrunt.hcl` file. Child configurations remain concise—often fewer than 20 lines of code:
```hcl
# In /live/prod/vpc/terragrunt.hcl:
include "root" {
  path = find_in_parent_folders()
}
```

### 2. `generate` (Dynamic File Synthesis)
Terragrunt can dynamically generate any arbitrary `.tf` file before executing Terraform. This permanently eliminates backend copy-pasting:
```hcl
# In Root terragrunt.hcl:
remote_state {
  backend = "s3"
  generate = {
    path      = "backend.tf"
    if_exists = "overwrite_terragrunt"
  }
  config = {
    bucket         = "enterprise-tfstate-${get_aws_account_id()}"
    key            = "${path_relative_to_include()}/terraform.tfstate" # Auto-generates unique state path!
    region         = "us-east-1"
    encrypt        = true
    dynamodb_table = "terraform-locks"
  }
}
```

### 3. `dependency` (First-Class Cross-Module Orchestration)
Terragrunt allows one module to directly declare a dependency on another module. Terragrunt fetches the dependency's outputs dynamically without requiring any `data "terraform_remote_state"` boilerplate:
```hcl
# In /live/prod/app/terragrunt.hcl:
dependency "vpc" {
  config_path = "../vpc"
}

inputs = {
  subnet_ids = dependency.vpc.outputs.private_subnet_ids
  vpc_id     = dependency.vpc.outputs.vpc_id
}
```

---

## 3. Production Enterprise File Hierarchy Layout

Terragrunt separates reusable modules from concrete environment configurations using the **"Live vs. Modules" pattern**:

```text
infrastructure-live/
├── terragrunt.hcl                 # ROOT CONFIG: Generates S3 backend & AWS provider
│
├── _envcommon/                    # SHARED COMPONENT INPUTS (DRY templates)
│   ├── vpc.hcl
│   └── eks.hcl
│
└── live/
    ├── dev/
    │   ├── env.hcl                # Environment-specific variables (account_id, env="dev")
    │   ├── vpc/
    │   │   └── terragrunt.hcl     # Points to remote module; inherits root & dev env
    │   └── eks/
    │       └── terragrunt.hcl     # Declares dependency on vpc; launches EKS
    │
    └── prod/
        ├── env.hcl                # account_id = "111122223333", env = "prod"
        ├── vpc/
        │   └── terragrunt.hcl
        └── eks/
            └── terragrunt.hcl
```

---

## 4. Multi-Module Bulk Orchestration: `run-all`

When managing complex multi-tier infrastructure, Terragrunt's **`run-all`** command executes operations across an entire directory tree, automatically respecting dependencies:

```bash
# Preview changes across the entire production infrastructure tree in dependency order:
terragrunt run-all plan

# Deploy VPC, databases, and EKS clusters in a single automated command:
terragrunt run-all apply

# Cleanly tear down an entire ephemeral staging environment in reverse dependency order:
terragrunt run-all destroy
```

### Concurrency and DAG Scheduling:
During `run-all apply`:
- Terragrunt builds a topological graph of all subdirectories based on their `dependency` blocks.
- Unrelated modules (e.g., `redis` and `postgres` that only depend on `vpc`) are deployed **simultaneously in parallel**.
- Dependent modules are held until their upstream dependencies complete successfully.

---

## 5. Architectural Comparison Matrix

| Capability / Dimension | Vanilla Terraform | Terragrunt | OpenTofu |
| :--- | :--- | :--- | :--- |
| **Backend Parameterization** | **Impossible** (Variables forbidden) | **100% Dynamic** via `path_relative_to_include()` | Partial (early variable evaluation) |
| **Multi-Directory Orchestration** | **None** (Requires custom bash scripts) | **Native** (`terragrunt run-all apply`) | None (Same as Terraform) |
| **Cross-Module Outputs** | Verbose `terraform_remote_state` | **Clean declarative `dependency` blocks** | Verbose `terraform_remote_state` |
| **Code Duplication (DRY)** | High (backend and provider copy-paste) | **Near Zero** (Inherited from root `terragrunt.hcl`)| Moderate |
| **CLI Learning Curve** | Low / Standard | Moderate (Requires learning HCL wrapper syntax)| Low / Standard |

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← Prev Module (07-Terraform-Workspaces-and-Environments)](../07-Terraform-Workspaces-and-Environments/12-Quick-Revision.md) | [Index](../../../README.md) | [02 - DRY Backend and Provider Configuration →](./02-DRY-Backend-and-Provider-Configuration.md) |
