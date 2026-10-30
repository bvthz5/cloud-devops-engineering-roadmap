# 07 - Real-World Scenarios

## Scenario 01: State File Corruption Recovery

### Incident
A CI pipeline crashed mid-apply, leaving a partially written state file on S3. Subsequent plans showed phantom resources that didn't exist.

### Resolution
1. Downloaded the corrupted state: `terraform state pull > corrupted.json`
2. Restored previous version from S3 bucket versioning
3. Ran `terraform plan` to identify orphaned resources
4. Used `terraform state rm` to clean phantom entries
5. Implemented CI pipeline retries with state locking timeout

---

## Scenario 02: Accidental State Deletion

### Incident
An engineer accidentally deleted the S3 state file thinking it was a backup.

### Resolution
1. Immediately stopped all `terraform apply` operations
2. Restored state from S3 versioning (bucket had versioning enabled)
3. If no versioning: rebuilt state using `terraform import` for each resource
4. **Preventive measure:** Enabled S3 Object Lock (WORM) and MFA Delete

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 06 - State Splitting and Multi State Architecture](./06-State-Splitting-and-Multi-State-Architecture.md) | [Index](../../../README.md) | [08 - Troubleshooting →](./08-Troubleshooting.md) |
