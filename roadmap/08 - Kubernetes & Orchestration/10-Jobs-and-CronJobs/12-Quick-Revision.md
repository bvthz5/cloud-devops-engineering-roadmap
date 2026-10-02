# 12 - Quick-Revision & Enterprise Cheat Sheet

## Job & CronJob Cheat Sheet

- **Restart Policy:** Must be `OnFailure` or `Never`.
- **Concurrency Policies:** `Forbid` (safe for DB/stateful), `Replace` (safe for cache), `Allow` (independent).
- **Auto-Cleanup:** `ttlSecondsAfterFinished: <seconds>`.
- **Hard Timeout:** `activeDeadlineSeconds: <seconds>`.
- **Retry Control:** `backoffLimit` & `podFailurePolicy`.

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [11 - MCQ](./11-MCQ.md) | [README](./README.md) | [Module 11 - Autoscaling](../11-Auto-Scaling-HPA-VPA-Cluster-Autoscaler/README.md) |
