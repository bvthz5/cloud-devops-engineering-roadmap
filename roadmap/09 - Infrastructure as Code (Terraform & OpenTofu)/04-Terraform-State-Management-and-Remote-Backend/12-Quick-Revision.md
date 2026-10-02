# 12 - Quick-Revision & Enterprise Cheat Sheet

## State Commands Quick Reference

| Command | Purpose |
|---|---|
| `terraform state list` | List all tracked resources |
| `terraform state show <resource>` | Show resource details |
| `terraform state mv` | Rename/move resource in state |
| `terraform state rm` | Remove from state (orphan) |
| `terraform state pull` | Download remote state |
| `terraform state push` | Upload state to remote |
| `terraform import` | Import existing resource |
| `terraform force-unlock` | Release stuck lock |

## Backend Comparison

| Backend | Locking | Encryption | Cross-Team |
|---|---|---|---|
| Local | ❌ | ❌ | ❌ |
| S3 + DynamoDB | ✅ | ✅ (KMS) | ✅ |
| GCS | ✅ (built-in) | ✅ (CMEK) | ✅ |
| AzureRM | ✅ (blob lease) | ✅ (SSE) | ✅ |

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [11 - MCQ](./11-MCQ.md) | [README](./README.md) | [Module 05 - Modules & Reusable Design](../05-Terraform-Modules-and-Reusable-Design/README.md) |
