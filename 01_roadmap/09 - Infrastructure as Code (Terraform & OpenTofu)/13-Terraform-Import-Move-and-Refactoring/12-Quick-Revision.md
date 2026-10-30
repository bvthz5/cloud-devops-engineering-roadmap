# 12 - Quick-Revision

## Import & Refactoring Commands

| Command/Block | Purpose |
|---|---|
| `terraform import` | CLI import existing resource |
| `import {}` block | Declarative import (>= 1.5) |
| `-generate-config-out` | Auto-generate HCL from import |
| `moved {}` block | Rename/relocate without destroy |
| `removed {}` block | Stop managing without destroy (>= 1.7) |
| `terraform state rm` | Remove from state (cloud resource stays) |
| `terraform state mv` | Move resource between states |

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 11 - MCQ](./11-MCQ.md) | [Index](../../../README.md) | [Next Module (14-CICD-for-Terraform-Atlantis-and-Pipelines) →](../14-CICD-for-Terraform-Atlantis-and-Pipelines/01-Terraform-CI-CD-Pipeline-Architecture.md) |
