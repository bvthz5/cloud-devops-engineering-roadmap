# 02 - moved Block Refactoring

> **Concept:** Safely restructuring, renaming, and modularizing Terraform code without triggering destructive recreate cycles (destroy and recreate) on active production infrastructure.

---

## 🚨 1. The Catastrophic Refactoring Bug in Early Terraform ($< 1.1$)

Before Terraform 1.1, changing a resource name or moving it into a module was **fatal**:

```hcl
# OLD CODE:
resource "aws_db_instance" "prod_database" { ... }

# REFACTORED CODE (Cleaner naming convention):
resource "aws_db_instance" "primary" { ... }
```

### What Terraform Did:
```text
Terraform Plan Output:
  - aws_db_instance.prod_database will be DESTROYED! 💥
  + aws_db_instance.primary will be CREATED! ⏳
```
Because Terraform maps resources strictly by their HCL address, it assumed the old database was deleted and a new one was requested. Running `apply` would **drop the multi-terabyte production database**!

---

## 🛡️ 2. The Solution: The `moved {}` Block (Terraform 1.1+ & OpenTofu)

The **`moved {}`** block instructs Terraform's state engine:
*"This is NOT a deletion! The resource formerly known as `A` has simply been moved to `B` in state."*

```text
WITHOUT moved BLOCK:
[ HCL: aws_db_instance.primary ] ──► Destroys old DB ──► Creates new DB (DOWNTIME!) 💥

WITH moved BLOCK:
┌────────────────────────────────────────┐
│ moved {                                │
│   from = aws_db_instance.prod_database │
│   to   = aws_db_instance.primary       │
│ }                                      │
└───────────────────┬────────────────────┘
                    │ Rewrites State In-Place! (ZERO CLOUD CHANGES)
                    ▼
[ State: aws_db_instance.primary points to the exact same live AWS RDS database! ] ✅
```

---

## 🛠️ 3. Core Refactoring Use Cases

### 3.1 Renaming a Standalone Resource
```hcl
# main.tf

# 1. New resource block name
resource "aws_instance" "api_server" {
  ami           = "ami-0c55b159cbfafe1f0"
  instance_type = "t3.large"
}

# 2. Inform Terraform of the rename
moved {
  from = aws_instance.web_server
  to   = aws_instance.api_server
}
```
- Output on `terraform plan`:
  ```text
  aws_instance.web_server has moved to aws_instance.api_server
  Plan: 0 to add, 0 to change, 0 to destroy.
  ```

---

### 3.2 Moving a Monolithic Resource into a Reusable Module
When decomposing a monolithic `main.tf` into a module:

```hcl
# Move root VPC into the networking module
moved {
  from = aws_vpc.main
  to   = module.networking.aws_vpc.this
}

# Move root Internet Gateway into the module
moved {
  from = aws_internet_gateway.gw
  to   = module.networking.aws_internet_gateway.this
}
```

---

### 3.3 Converting a Single Resource to `for_each` or `count`
Moving from a single resource to a multi-instance map or array without recreation:

```hcl
# Converting single subnet to a for_each map
moved {
  from = aws_subnet.public
  to   = aws_subnet.public["us-east-1a"]
}

# Converting single VM to a count array
moved {
  from = aws_instance.app
  to   = aws_instance.app[0]
}
```

---

### 3.4 Module-to-Module Restructuring
When migrating resources between two child modules:

```hcl
moved {
  from = module.legacy_stack.aws_s3_bucket.assets
  to   = module.storage_tier.aws_s3_bucket.static_assets
}
```

---

## 📋 4. Best Practices & Lifecycle of `moved` Blocks

1. **Keep for At Least One Full Deployment Cycle:**
   - In team environments, keep `moved {}` blocks in code until all developers, staging environments, and production CI/CD pipelines have run `terraform apply`.
2. **Safe Deletion After Migration:**
   - Once the production state has been updated, `moved {}` blocks can be safely deleted in a subsequent commit.
3. **Module Authors Must Ship `moved` Blocks:**
   - If you maintain an internal Terraform module used by 50 engineering teams, include `moved {}` blocks in version upgrades (e.g. `v2.0.0`) so consumers can upgrade seamlessly with zero downtime!

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 01 - terraform import CLI and Import Block](./01-terraform-import-CLI-and-Import-Block.md) | [Index](../../../README.md) | [03 - removed Block and State Surgery →](./03-removed-Block-and-State-Surgery.md) |
