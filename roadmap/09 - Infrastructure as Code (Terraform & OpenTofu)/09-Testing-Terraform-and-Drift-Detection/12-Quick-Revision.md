# 12 - Quick-Revision & Enterprise Cheat Sheet

## Testing Tool Quick Reference

| Tool | Type | Language | Speed |
|---|---|---|---|
| `terraform fmt` | Formatting | N/A | Instant |
| `terraform validate` | Syntax check | N/A | Instant |
| `tflint` | Linting | HCL rules | Instant |
| `checkov` / `tfsec` | Security scan | Python/Go | Fast |
| `terraform test` | Unit/Integration | HCL | Medium |
| `Terratest` | Integration | Go | Slow |
| `conftest` | Policy | Rego | Fast |

## Drift Detection

| Exit Code | Meaning | Action |
|---|---|---|
| 0 | No drift | All clear |
| 1 | Error | Investigate |
| 2 | Drift detected | Alert + remediate |

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [11 - MCQ](./11-MCQ.md) | [README](./README.md) | [Module 10 - IaC Security](../10-Terraform-Best-Practices-and-Security/README.md) |
