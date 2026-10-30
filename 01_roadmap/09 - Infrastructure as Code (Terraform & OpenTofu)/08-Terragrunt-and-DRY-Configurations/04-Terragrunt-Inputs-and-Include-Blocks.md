# 04 - Terragrunt Inputs & Include Blocks

## 1. Hierarchical Configuration

```hcl
# live/dev/terragrunt.hcl (environment level)
locals {
  environment = "dev"
  account_id  = "123456789012"
}

inputs = {
  environment = local.environment
  account_id  = local.account_id
}
```

## 2. Include Blocks

```hcl
# live/dev/vpc/terragrunt.hcl
include "root" {
  path   = find_in_parent_folders()
  expose = true   # Expose parent config to this module
}

include "env" {
  path   = find_in_parent_folders("env.hcl")
  expose = true
}
```

## 3. Reading External Config

```hcl
locals {
  account_vars = read_terragrunt_config(find_in_parent_folders("account.hcl"))
  region_vars  = read_terragrunt_config(find_in_parent_folders("region.hcl"))
  env_vars     = read_terragrunt_config(find_in_parent_folders("env.hcl"))
}

inputs = {
  account_id = local.account_vars.locals.account_id
  region     = local.region_vars.locals.region
  env        = local.env_vars.locals.environment
}
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 03 - Dependency Management and Orchestration](./03-Dependency-Management-and-Orchestration.md) | [Index](../../../README.md) | [05 - Multi Environment with Terragrunt →](./05-Multi-Environment-with-Terragrunt.md) |
