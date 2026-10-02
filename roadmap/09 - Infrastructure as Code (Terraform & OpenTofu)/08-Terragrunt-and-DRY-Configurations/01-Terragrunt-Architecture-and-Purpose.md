# 01 - Terragrunt Architecture & Purpose

## 1. What Is Terragrunt?

Terragrunt is a thin wrapper around Terraform/OpenTofu that provides tools for keeping configurations DRY (Don't Repeat Yourself), managing remote state, and orchestrating multi-module deployments.

```text
+--------------------------------------------------+
|               TERRAGRUNT WRAPPER                  |
|                                                    |
|  terragrunt.hcl (config)                          |
|       |                                            |
|       v                                            |
|  Generates: backend.tf, provider.tf               |
|  Passes: inputs as TF_VAR_* environment vars      |
|  Orchestrates: dependency ordering                 |
|       |                                            |
|       v                                            |
|  terraform init / plan / apply                     |
+--------------------------------------------------+
```

## 2. Problems Terragrunt Solves

| Problem | Without Terragrunt | With Terragrunt |
|---|---|---|
| Backend config duplication | Copy backend block to every module | Define once in root terragrunt.hcl |
| Provider config duplication | Copy provider block everywhere | Generate dynamically |
| Cross-module dependencies | Manual apply order | `dependency` blocks with auto-ordering |
| Multi-environment management | Duplicate directory trees | Hierarchical config with `include` |
| Bulk operations | `cd` into each module and apply | `terragrunt run-all apply` |

## 3. File Layout

```text
live/
+-- terragrunt.hcl           <- Root config (backend, provider generation)
+-- dev/
|   +-- terragrunt.hcl       <- Environment config (inherits root)
|   +-- vpc/
|   |   +-- terragrunt.hcl   <- Module config (source, inputs)
|   +-- ecs/
|       +-- terragrunt.hcl
+-- prod/
    +-- terragrunt.hcl
    +-- vpc/
    |   +-- terragrunt.hcl
    +-- ecs/
        +-- terragrunt.hcl
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [README](./README.md) | [README](./README.md) | [02 - DRY Backend](./02-DRY-Backend-and-Provider-Configuration.md) |
