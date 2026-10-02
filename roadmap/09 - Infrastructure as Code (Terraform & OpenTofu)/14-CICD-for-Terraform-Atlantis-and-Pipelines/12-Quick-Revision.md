# 12 - Quick-Revision

## CI/CD Pipeline Pattern

| Stage | PR (Feature Branch) | Merge (Main Branch) |
|---|---|---|
| Format | `terraform fmt -check` | - |
| Validate | `terraform validate` | - |
| Lint | `tflint` | - |
| Security | `tfsec` / `checkov` | - |
| Plan | `terraform plan` (comment on PR) | `terraform plan -out=tfplan` |
| Apply | - | `terraform apply tfplan` |
| Notify | - | Slack/Teams notification |

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [11 - MCQ](./11-MCQ.md) | [README](./README.md) | [Module 15 - Terraform at Scale](../15-Terraform-at-Scale-Monorepo-and-Multi-Team/README.md) |
