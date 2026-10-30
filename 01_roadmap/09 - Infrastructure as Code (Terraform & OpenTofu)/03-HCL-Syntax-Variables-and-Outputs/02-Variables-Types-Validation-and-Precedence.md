# 02 - Variables: Types, Validation & Precedence

## 1. What Are Input Variables in Terraform?

In Terraform and OpenTofu, **Input Variables** serve as the parameters of your infrastructure code. They allow a single set of declarative HCL blueprints to be customized and reused across multiple environments, cloud regions, and business units without modifying the underlying resource definitions.

Variables define a strict typed interface for modules. When an engineer or CI/CD pipeline invokes Terraform, variables can be passed via files, environment variables, or command-line flags.

```text
+---------------------------------------------------------------------------------------------------+
|                        THE VARIABLE INPUT PARAMETERIZATION WORKFLOW                               |
+---------------------------------------------------------------------------------------------------+

 INPUT INGESTION CHANNELS:
 ┌──────────────────────┐   ┌──────────────────────┐   ┌──────────────────────┐
 │ TF_VAR_environment  │   │ terraform.tfvars     │   │ -var="region=us-east"│
 └──────────┬───────────┘   └──────────┬───────────┘   └──────────┬───────────┘
            │                          │                          │
            └──────────────────────────┼──────────────────────────┘
                                       ▼ Evaluated by Precedence Engine
 ┌──────────────────────────────────────────────────────────────────────────────────┐
 │ Variable Definition: variable "instance_type" {}                                 │
 │  ├── Validates Type Constraint: type = string                                    │
 │  ├── Runs Validation Rules: can(regex("^t3\\.", var.instance_type))              │
 │  └── Injects Value into HCL Resource: resource "aws_instance" "web" {}           │
 └──────────────────────────────────────────────────────────────────────────────────┘
```

---

## 2. The 7-Tier Variable Precedence Hierarchy

When the same variable is defined across multiple locations, Terraform resolves conflicts using a strict, deterministic **Precedence Hierarchy**. 

The hierarchy is ranked below from **Lowest Priority (1)** to **Highest Priority (7)**:

```text
+---------------------------------------------------------------------------------------------------+
|                        VARIABLE PRECEDENCE PYRAMID (LOWEST TO HIGHEST)                            |
+---------------------------------------------------------------------------------------------------+

                          ▲
                         / \
                        /   \
                       /  7  \   HIGHEST PRIORITY: CLI Flag: -var="name=value"
                      /───────\
                     /    6    \  CLI Flag: -var-file="secret.tfvars"
                    /───────────\
                   /      5      \ Any *.auto.tfvars files (Lexical Alphabetical Order)
                  /───────────────\
                 /        4        \ terraform.tfvars.json (Auto-Loaded)
                /───────────────────\
               /          3          \ terraform.tfvars (Auto-Loaded)
              /───────────────────────\
             /            2            \ Environment Variables: TF_VAR_<name>
            /───────────────────────────\
           /              1              \ LOWEST PRIORITY: "default" in variable block
          └───────────────────────────────┘
```

### Detailed Breakdown of Precedence Levels:
1. **Default Value (Lowest Priority):** The `default = "..."` argument declared inside the `variable` block. If no other source provides a value, this fallback is used.
2. **Environment Variables (`TF_VAR_<name>`):** Any environment variable prefixed with `TF_VAR_`. For example, `export TF_VAR_region="us-west-2"` sets `var.region`.
3. **`terraform.tfvars`:** A standard file named exactly `terraform.tfvars` located in the root module directory. Automatically loaded by Terraform without flags.
4. **`terraform.tfvars.json`:** JSON-formatted variable file, automatically loaded.
5. **`*.auto.tfvars` and `*.auto.tfvars.json`:** Any file ending with `.auto.tfvars`. Automatically loaded in alphabetical lexical order (e.g., `z.auto.tfvars` overrides `a.auto.tfvars`).
6. **Command-Line `-var-file` Flag:** Explicitly specifying a file path: `terraform apply -var-file="environments/prod.tfvars"`.
7. **Command-Line `-var` Flag (Highest Priority):** Passing values directly via the CLI: `terraform apply -var="instance_type=c6i.4xlarge"`. This overrides all files and environment variables.

---

## 3. The Terraform Type System

Terraform enforces a robust, compile-time static type system that prevents invalid configurations before cloud APIs are contacted:

### 1. Primitive Types:
- **`string`:** UTF-8 text strings (`"ami-0c55b159cbfafe1f0"`).
- **`number`:** Integer or floating-point numbers (`42`, `3.14159`).
- **`bool`:** Boolean values (`true` or `false`).

### 2. Collection Types (Homogeneous Elements):
- **`list(type)`:** An ordered sequence of elements of the same type: `list(string)`.
- **`set(type)`:** An unordered collection of unique elements with no duplicates: `set(string)`.
- **`map(type)`:** A collection of key-value pairs where all values share the same type: `map(string)`.

### 3. Structural Types (Heterogeneous Elements):
- **`object({...})`:** A named schema containing attributes of different types.
- **`tuple([...])`:** A fixed-length ordered list where each position has a specific type: `tuple([string, number, bool])`.

---

## 4. Modern Features: Optional Object Attributes (Terraform 1.3+)

Historically, defining complex object variables required callers to provide every single attribute, even if the value was empty or optional. 

Terraform 1.3 introduced the **`optional()` modifier**, allowing developers to declare optional object attributes with clean, built-in fallback defaults:

```hcl
variable "database_cluster" {
  type = object({
    cluster_name  = string
    instance_type = string
    node_count    = optional(number, 3)              # Defaults to 3 if omitted
    storage_gb    = optional(number, 100)            # Defaults to 100 if omitted
    backup_retention_days = optional(number, 7)      # Defaults to 7 if omitted
    enable_deletion_protection = optional(bool, true) # Defaults to true
  })
}
```

Now a caller can provide the minimal required configuration without verbose boilerplate:
```hcl
database_cluster = {
  cluster_name  = "production-postgres"
  instance_type = "db.r6g.xlarge"
  # All other fields cleanly adopt their declared defaults!
}
```

---

## 5. Custom Validation Rules (`validation {}` Blocks)

To prevent misconfigurations from causing runtime cloud errors, production modules declare custom validation rules that execute during `terraform plan`:

```hcl
variable "environment" {
  type        = string
  description = "Target deployment environment tier."

  validation {
    condition     = contains(["dev", "staging", "production"], var.environment)
    error_message = "The environment variable must be exactly 'dev', 'staging', or 'production'."
  }
}

variable "cidr_block" {
  type        = string
  description = "IPv4 CIDR block for the Virtual Private Cloud."

  validation {
    # Validates valid CIDR formatting using built-in cidrnetmask and can()
    condition     = can(cidrnetmask(var.cidr_block))
    error_message = "The cidr_block must be a valid IPv4 CIDR notation (e.g., 10.0.0.0/16)."
  }

  validation {
    # Enforces subnet sizing boundaries
    condition     = tonumber(split("/", var.cidr_block)[1]) <= 20
    error_message = "VPC CIDR block must be /20 or larger (e.g. /16) to ensure sufficient address space."
  }
}
```

---

## 6. Sensitive Variables and Nullable Controls

### 1. `sensitive = true` (Protecting Secrets in Terminal Logs)
```hcl
variable "db_password" {
  type        = string
  description = "Master password for the database."
  sensitive   = true
}
```
When a variable is marked `sensitive = true`, Terraform masks its value in CLI outputs and execution plans, displaying `(sensitive value)` instead of the plaintext password.

### 2. `nullable = false` (Preventing Undefined State)
By default, passing `null` to a variable without a default causes Terraform to accept `null`. Setting `nullable = false` guarantees that even if a caller explicitly passes `null`, Terraform rejects the input and requires a valid value.

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 01 - HCL Language Fundamentals Blocks Arguments Expressions](./01-HCL-Language-Fundamentals-Blocks-Arguments-Expressions.md) | [Index](../../../README.md) | [03 - Outputs Data Sources and Cross Module References →](./03-Outputs-Data-Sources-and-Cross-Module-References.md) |
