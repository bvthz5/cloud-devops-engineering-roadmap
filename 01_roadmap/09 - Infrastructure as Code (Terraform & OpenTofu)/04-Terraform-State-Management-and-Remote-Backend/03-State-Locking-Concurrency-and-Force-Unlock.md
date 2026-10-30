# 03 - State Locking, Concurrency & Force-Unlock

## 1. The Concurrency Problem: Lost Updates and Torn Writes

In distributed systems, when multiple clients attempt to modify a shared database concurrently without mutual exclusion, they cause **race conditions, lost updates, and torn writes**.

Consider what occurs in a team environment without state locking:
1. **10:00:00 AM:** Engineer Alice runs `terraform apply` to create a Kubernetes cluster. Terraform reads state version 10.
2. **10:00:15 AM:** Engineer Bob runs `terraform apply` to add an S3 bucket. Bob's Terraform process also reads state version 10.
3. **10:00:45 AM:** Bob's S3 bucket finishes creating in 30 seconds. Bob's process writes state version 11 to S3, recording the new bucket.
4. **10:15:00 AM:** Alice's Kubernetes cluster finishes creating after 15 minutes. Alice's process writes its updated state back to S3.
5. **The Catastrophe:** Alice's write completely **overwrites and obliterates** Bob's state version 11. The S3 bucket exists in the cloud, but Terraform has forgotten it permanently. The state has suffered a silent lost update!

**State Locking** prevents this concurrency disaster by acquiring an exclusive, distributed mutex lock before any operation that reads or writes state.

```text
+---------------------------------------------------------------------------------------------------+
|                        THE STATE LOCKING MUTEX PROTOCOL                                           |
+---------------------------------------------------------------------------------------------------+

 Engineer Alice Runs "terraform apply"          Engineer Bob Runs "terraform apply"
 ┌───────────────────────────────────┐          ┌───────────────────────────────────┐
 │ Acquires Exclusive Lock via       │          │ Attempts to Acquire Lock          │
 │ DynamoDB Conditional PutItem:     │          │ Query: PutItem(LockID="vpc.state")│
 │ PutItem if attribute_not_exists   │          └─────────────────┬─────────────────┘
 └─────────────────┬─────────────────┘                            │
                   │ Lock Granted!                                │ 400 ConditionalCheckFailedException!
                   ▼                                              ▼
 ┌───────────────────────────────────┐                  ┌───────────────────────────────────┐
 │ DynamoDB Table: LockID Active     │                  │ EXECUTION HALTED!                 │
 │ Writes state updates safely...    │                  │ "Error acquiring the state lock:  │
 └─────────────────┬─────────────────┘                  │  Lock held by Alice at 10:00 AM"  │
                   │                                    └───────────────────────────────────┘
                   ▼ Apply Finishes
 ┌───────────────────────────────────┐
 │ Deletes Lock (DeleteItem)         │
 └───────────────────────────────────┘
```

---

## 2. Under the Hood: DynamoDB Lock Table Mechanics

When using the AWS S3 backend, state locking is enforced by an Amazon DynamoDB table. The DynamoDB table must be created with a strict schema:
- **Primary Partition Key:** Must be named exactly **`LockID`** with type **String (`S`)**.

### The Lock JSON Payload:
When Terraform locks the state, it executes an atomic DynamoDB `PutItem` API call with a conditional expression (`attribute_not_exists(LockID)`). It writes a detailed diagnostic JSON payload into the `Info` attribute:

```json
{
  "LockID": "enterprise-production-terraform-state-us-east-1/core-platform/networking/terraform.tfstate-md5",
  "ID": "a1b2c3d4-e5f6-7890-abcd-ef1234567890",
  "Operation": "OperationTypeApply",
  "Info": "",
  "Who": "alice@corporate-macbook-pro.local",
  "Version": "1.8.0",
  "Created": "2024-03-15T10:00:15.123456Z",
  "Path": "enterprise-production-terraform-state-us-east-1/core-platform/networking/terraform.tfstate"
}
```

If another process attempts to write while `LockID` exists, DynamoDB returns a `ConditionalCheckFailedException`, and the second Terraform process halts with an informative error message.

---

## 3. The Stale / Orphaned Lock Crisis

An **Orphaned Lock** occurs when the process holding the lock terminates abnormally before it can execute the `DeleteItem` cleanup call.

### Common Root Causes of Orphaned Locks:
1. **Abrupt Process Termination:** The engineer pressed `Ctrl+C` multiple times, or the OS kernel killed the process via `SIGKILL` (Out of Memory).
2. **CI/CD Pipeline Cancellation:** A developer cancelled a running GitHub Actions or GitLab CI workflow midway through an apply. The container runner was forcefully terminated, leaving the DynamoDB lock intact.
3. **Network or Power Outage:** The workstation's Wi-Fi dropped or laptop battery died while Terraform was waiting for a long-running cloud resource.

When this occurs, any subsequent run fails immediately:

```text
Error: Error acquiring the state lock

Lock Info:
  ID:        a1b2c3d4-e5f6-7890-abcd-ef1234567890
  Path:      enterprise-production-terraform-state-us-east-1/networking.tfstate
  Operation: OperationTypeApply
  Who:       alice@corporate-macbook-pro.local
  Version:   1.8.0
  Created:   2024-03-15 10:00:15 UTC
  Info:      

Terraform acquires a state lock to protect the state from being written
by multiple users at the same time. Please resolve the issue above and try
again.
```

---

## 4. The Dangerous `terraform force-unlock` Command

To clear an orphaned lock, Terraform provides the **`force-unlock`** command:

$$\text{Syntax: } \texttt{terraform force-unlock <LOCK\_ID>}$$

```bash
terraform force-unlock a1b2c3d4-e5f6-7890-abcd-ef1234567890
```

### The SRE Pre-Flight Verification Checklist (MANDATORY):
> [!CAUTION]
> **Running `force-unlock` while another process is genuinely running will cause irreversible state corruption and duplicate cloud resources.**

Before executing `force-unlock`, verify all three conditions:
1. **Verify the Process Is Dead:** Contact the person listed in the `Who:` field (`alice@corporate-macbook-pro.local`). Confirm that their terminal is closed and no local process is running.
2. **Check CI/CD Logs:** If the lock was created by a CI/CD service account, verify that the pipeline job has finished, failed, or been completely cancelled.
3. **Inspect Creation Timestamp:** If the lock was created 3 minutes ago, wait. Cloud resources like AWS RDS or EKS clusters legitimately take 15 to 20 minutes to provision. Only consider a lock orphaned if the holder is confirmed dead or the timestamp is hours old.

---

## 5. Automated Lock Wait: `-lock-timeout`

In automated CI/CD pipelines where multiple developers may merge pull requests within minutes of each other, failing immediately upon finding a locked state creates unnecessary pipeline failures.

You can configure Terraform to patiently wait for an active lock to be released using the **`-lock-timeout`** flag:

```bash
# Instructs Terraform to retry acquiring the lock for up to 15 minutes before failing:
terraform plan -lock-timeout=15m
terraform apply -lock-timeout=15m
```

During this timeout period, Terraform polls the backend periodically. As soon as the preceding job finishes and releases the lock, the waiting job acquires the lock and proceeds seamlessly.

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 02 - Remote State Backends S3 GCS AzureRM Consul](./02-Remote-State-Backends-S3-GCS-AzureRM-Consul.md) | [Index](../../../README.md) | [04 - State Commands mv rm import taint untaint →](./04-State-Commands-mv-rm-import-taint-untaint.md) |
