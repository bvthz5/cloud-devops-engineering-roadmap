# 12 - Quick-Revision & Enterprise Cheat Sheet

## Terragrunt Commands

| Command | Purpose |
|---|---|
| `terragrunt run-all plan` | Plan all modules respecting dependencies |
| `terragrunt run-all apply` | Apply all modules in dependency order |
| `terragrunt run-all destroy` | Destroy all in reverse dependency order |
| `terragrunt graph-dependencies` | Show module dependency graph |

## Key Functions

| Function | Purpose |
|---|---|
| `find_in_parent_folders()` | Find parent terragrunt.hcl |
| `path_relative_to_include()` | Relative path for unique state keys |
| `read_terragrunt_config()` | Read and parse external .hcl config |

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [11 - MCQ](./11-MCQ.md) | [README](./README.md) | [Module 09 - Testing IaC](../09-Testing-Terraform-and-Drift-Detection/README.md) |
