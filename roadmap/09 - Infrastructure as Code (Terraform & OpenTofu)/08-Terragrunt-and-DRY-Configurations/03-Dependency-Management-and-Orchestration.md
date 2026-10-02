# 03 - Dependency Management & Orchestration

## 1. Cross-Module Dependencies

```hcl
# live/dev/ecs/terragrunt.hcl
dependency "vpc" {
  config_path = "../vpc"
  mock_outputs = {
    vpc_id     = "vpc-mock123"
    subnet_ids = ["subnet-mock1", "subnet-mock2"]
  }
}

inputs = {
  vpc_id     = dependency.vpc.outputs.vpc_id
  subnet_ids = dependency.vpc.outputs.subnet_ids
}
```

## 2. Execution Order

```text
terragrunt run-all apply
    |
    +-- 1. vpc (no dependencies)
    +-- 2. security-groups (depends on vpc)
    +-- 3. rds (depends on vpc, security-groups)
    +-- 4. ecs (depends on vpc, rds)
```

Terragrunt automatically resolves the DAG and applies in correct order.

## 3. Mock Outputs

Mock outputs allow `terragrunt plan` to work even when dependencies haven't been applied yet, enabling parallelized CI plans.

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [02 - DRY Backend](./02-DRY-Backend-and-Provider-Configuration.md) | [README](./README.md) | [04 - Inputs & Include](./04-Terragrunt-Inputs-and-Include-Blocks.md) |
