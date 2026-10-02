# 03 - State Locking, Concurrency & Force-Unlock

## 1. Why State Locking?

Without locking, two engineers running `terraform apply` simultaneously could corrupt the state file — both read the same state, both make changes, and the last to write wins (overwriting the other's changes).

## 2. Locking Implementation by Backend

| Backend | Locking Mechanism | Configuration |
|---|---|---|
| S3 | DynamoDB table | `dynamodb_table = "terraform-locks"` |
| GCS | Built-in (Google Cloud native) | Automatic |
| AzureRM | Blob lease | Automatic |
| Consul | Consul sessions | Automatic |
| Terraform Cloud | Built-in | Automatic |
| Local | OS file lock | Automatic (single machine only) |

## 3. Lock Conflict Resolution

```bash
# When you see:
# Error: Error acquiring the state lock
# Lock Info:
#   ID:        a1b2c3d4-e5f6-7890
#   Path:      s3://bucket/key/terraform.tfstate
#   Operation: OperationTypeApply
#   Who:       engineer@laptop
#   Created:   2024-01-15 10:30:00 UTC

# Step 1: Verify the other operation is truly stale
# (Ask the person, check CI logs)

# Step 2: Force unlock (DANGEROUS — only if confirmed stale)
terraform force-unlock a1b2c3d4-e5f6-7890
```

## 4. Best Practices

1. **Never force-unlock without verification** — confirm the holding process is dead
2. **Use CI/CD pipelines** — single point of `apply` eliminates most lock conflicts
3. **Keep apply operations short** — split large states to reduce lock hold time
4. **Set lock timeouts** — `terraform plan -lock-timeout=5m`

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [02 - Remote Backends](./02-Remote-State-Backends-S3-GCS-AzureRM-Consul.md) | [README](./README.md) | [04 - State Commands](./04-State-Commands-mv-rm-import-taint-untaint.md) |
