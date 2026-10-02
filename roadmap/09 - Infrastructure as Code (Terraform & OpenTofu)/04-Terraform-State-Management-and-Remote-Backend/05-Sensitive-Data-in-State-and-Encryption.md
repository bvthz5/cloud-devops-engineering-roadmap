# 05 - Sensitive Data in State & Encryption

## 1. The State File Contains Secrets

**Critical:** The Terraform state file stores ALL resource attributes in plaintext, including:
- Database passwords
- API keys
- TLS private keys
- Access tokens

## 2. Protection Strategies

| Strategy | Protection Level |
|---|---|
| Remote backend with encryption at rest | ✅ Encrypted on disk (S3 SSE, GCS CMEK) |
| Remote backend with encryption in transit | ✅ TLS for all API calls |
| `sensitive = true` on outputs | ✅ Hidden from CLI output (still in state!) |
| OpenTofu client-side encryption | ✅✅ State encrypted BEFORE upload to backend |
| Terraform Cloud | ✅✅ Enterprise-grade encryption + RBAC |
| Vault-managed secrets | ✅✅✅ Secrets never enter state (dynamic credentials) |

## 3. Sensitive Variables and Outputs

```hcl
variable "db_password" {
  type      = string
  sensitive = true    # Won't show in plan/apply output
}

output "db_connection_string" {
  value     = "postgres://admin:${var.db_password}@${aws_db_instance.main.endpoint}/mydb"
  sensitive = true    # Won't show in CLI (still in state file!)
}
```

## 4. Best Practices for State Security

1. **Never commit state files to Git** — add `*.tfstate` to `.gitignore`
2. **Enable S3 bucket versioning** — recover from accidental overwrites
3. **Enable S3 bucket encryption** — KMS or SSE-S3
4. **Restrict IAM access** — only CI/CD service accounts can read/write state
5. **Enable DynamoDB point-in-time recovery** — backup lock table
6. **Use dynamic secrets** (Vault/AWS STS) — credentials auto-rotate, never stored permanently

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 04 - State Commands mv rm import taint untaint](./04-State-Commands-mv-rm-import-taint-untaint.md) | [Index](../../../README.md) | [06 - State Splitting and Multi State Architecture →](./06-State-Splitting-and-Multi-State-Architecture.md) |
