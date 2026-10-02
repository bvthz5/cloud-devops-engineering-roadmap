# 02 - Workspaces vs Directory-Based Environments

## 1. Strategy Comparison

| Aspect | Workspaces | Directory-Based |
|---|---|---|
| **Code** | Single copy of .tf files | Duplicated or shared via modules |
| **State** | Automatic separation by workspace name | Explicit separate backends |
| **Divergence** | Hard to make envs different (same code) | Easy to customize per environment |
| **Complexity** | Simple (one directory) | More directories to manage |
| **Blast Radius** | Workspace switch mistake = wrong env | Directories are physically separate |
| **Best For** | Identical environments (testing/sandboxes) | Environments with different configs |

## 2. Directory-Based Pattern

```text
infrastructure/
+-- modules/              <- Shared reusable modules
|   +-- vpc/
|   +-- compute/
+-- environments/
    +-- dev/
    |   +-- main.tf       <- Calls modules with dev values
    |   +-- terraform.tf  <- Dev backend config
    |   +-- dev.tfvars
    +-- staging/
    |   +-- main.tf
    |   +-- terraform.tf  <- Staging backend config
    |   +-- staging.tfvars
    +-- prod/
        +-- main.tf
        +-- terraform.tf  <- Prod backend config
        +-- prod.tfvars
```

## 3. Recommendation

| Scenario | Recommended Strategy |
|---|---|
| Dev/staging/prod with different configs | Directory-based |
| Ephemeral test environments | Workspaces |
| Multi-region identical deployments | Workspaces or for_each with provider aliases |
| Enterprise with strict change control | Directory-based (separate state, separate PRs) |

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 01 - Terraform Workspaces Architecture and CLI](./01-Terraform-Workspaces-Architecture-and-CLI.md) | [Index](../../../README.md) | [03 - Environment Segregation Patterns →](./03-Environment-Segregation-Patterns.md) |
