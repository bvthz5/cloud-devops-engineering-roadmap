# 12 - Quick-Revision & Enterprise Cheat Sheet

## Security Scanning Tools

| Tool | Focus | Integration |
|---|---|---|
| tfsec / Trivy | Security misconfigs | CLI, CI, pre-commit |
| Checkov | Compliance + security | CLI, CI, IDE plugins |
| Conftest/OPA | Custom policies on plan JSON | CI pipeline |
| Sentinel | Enterprise policies | Terraform Cloud only |

## Secret Management Options

| Tool | Use Case |
|---|---|
| HashiCorp Vault | Dynamic secrets, multi-cloud |
| AWS Secrets Manager | AWS-native, auto-rotation |
| AWS SSM Parameter Store | Simple key-value secrets |
| SOPS | Encrypt files in Git |
| Azure Key Vault | Azure-native secrets |
| GCP Secret Manager | GCP-native secrets |

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 11 - MCQ](./11-MCQ.md) | [Index](../../../README.md) | [Next Module (11-Terraform-Cloud-and-Enterprise) →](../11-Terraform-Cloud-and-Enterprise/01-Terraform-Cloud-Architecture-and-Pricing-Tiers.md) |
