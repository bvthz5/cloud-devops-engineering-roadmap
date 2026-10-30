# 05 - Multi-Environment with Terragrunt

## 1. Enterprise Layout

```text
live/
+-- terragrunt.hcl            <- Root (backend, provider)
+-- _envcommon/                <- Shared module configs
|   +-- vpc.hcl
|   +-- ecs.hcl
+-- dev/
|   +-- env.hcl               <- environment = "dev"
|   +-- account.hcl           <- account_id = "111..."
|   +-- us-east-1/
|       +-- region.hcl        <- region = "us-east-1"
|       +-- vpc/
|       |   +-- terragrunt.hcl
|       +-- ecs/
|           +-- terragrunt.hcl
+-- prod/
    +-- env.hcl               <- environment = "prod"
    +-- account.hcl           <- account_id = "222..."
    +-- us-east-1/
        +-- region.hcl
        +-- vpc/
        |   +-- terragrunt.hcl
        +-- ecs/
            +-- terragrunt.hcl
```

## 2. Deploying an Entire Environment

```bash
# Deploy all modules in dev
cd live/dev
terragrunt run-all apply

# Plan all modules in prod
cd live/prod
terragrunt run-all plan

# Destroy all modules in dev
cd live/dev
terragrunt run-all destroy
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 04 - Terragrunt Inputs and Include Blocks](./04-Terragrunt-Inputs-and-Include-Blocks.md) | [Index](../../../README.md) | [06 - Terragrunt run all and CI CD Integration →](./06-Terragrunt-run-all-and-CI-CD-Integration.md) |
