# 05 - Concurrency Policies: Allow, Forbid, and Replace

## 1. The Concurrency Problem

If a job is scheduled to run every 5 minutes (`*/5 * * * *`), but an execution encounters slow database locks and takes 18 minutes to complete, what should Kubernetes do when the next 5-minute interval arrives?

---

## 2. The Three Concurrency Policies

| Policy | Behavior on Overlap | Recommended For |
|---|---|---|
| **`Allow`** (Default) | Runs concurrently alongside the existing job. | Independent stateless queries |
| **`Forbid`** | Skips the new execution until the current one finishes. | **Database backups, ledger reconciliations, stateful batch jobs** |
| **`Replace`** | Cancels the currently running job and starts the new one. | Stale report generation, cache warmups |

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 04 - CronJob Architecture and Schedule Syntax](./04-CronJob-Architecture-and-Schedule-Syntax.md) | [Index](../../../README.md) | [06 - History Limits CleanUp and Deadlines →](./06-History-Limits-CleanUp-and-Deadlines.md) |
