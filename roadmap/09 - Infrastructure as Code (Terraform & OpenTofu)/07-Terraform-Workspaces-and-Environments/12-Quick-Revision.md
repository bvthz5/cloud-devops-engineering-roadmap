# 12 - Quick-Revision & Enterprise Cheat Sheet

## Environment Strategy Decision

| Scenario | Strategy |
|---|---|
| Identical environments | Workspaces |
| Different configs per env | Directory-based |
| Enterprise multi-account | Directory + provider aliases + assume_role |
| Ephemeral test envs | Workspaces |

## Workspace Commands

| Command | Purpose |
|---|---|
| `terraform workspace list` | List all workspaces |
| `terraform workspace new <name>` | Create workspace |
| `terraform workspace select <name>` | Switch workspace |
| `terraform workspace show` | Show current workspace |
| `terraform workspace delete <name>` | Delete workspace |

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [11 - MCQ](./11-MCQ.md) | [README](./README.md) | [Module 08 - Terragrunt](../08-Terragrunt-and-DRY-Configurations/README.md) |
