# 09 - Interview Questions & Scenarios

### Q1: What does the Terraform state file contain and why is it important?
**Answer:**
The state file is a JSON document mapping HCL configuration to real cloud resources. It contains resource IDs, attributes, dependencies, outputs, and metadata (serial, lineage). It's essential because Terraform uses it to determine what exists, compute diffs for plans, and track resource ownership.

---

### Q2: How do you handle state file corruption in production?
**Answer:**
1. Stop all apply operations immediately
2. Restore from S3 versioning or GCS object versioning
3. If no backup: use `terraform import` to rebuild state from existing cloud resources
4. Validate with `terraform plan` — should show no changes if state is correct
5. Prevent recurrence: enable bucket versioning, object lock, and MFA delete

---

### Q3: Explain state locking and what happens if it fails.
**Answer:**
State locking prevents concurrent writes that could corrupt the state. For S3, a DynamoDB table records a lock entry before any write. If locking fails (stale lock from crashed process), you can `terraform force-unlock <LOCK_ID>` after confirming no other process is running.

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 08 - Troubleshooting](./08-Troubleshooting.md) | [Index](../../../README.md) | [10 - Hands On Practice →](./10-Hands-On-Practice.md) |
