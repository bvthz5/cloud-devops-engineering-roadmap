# 04 - CronJob Architecture and Schedule Syntax

## 1. CronJob Mechanics

A **CronJob** runs a Job periodically on a given schedule, written in standard Unix cron format.

```text
Cron Syntax:
  ┌───────────── minute (0 - 59)
  │ ┌───────────── hour (0 - 23)
  │ │ ┌───────────── day of the month (1 - 31)
  │ │ │ ┌───────────── month (1 - 12)
  │ │ │ │ ┌───────────── day of the week (0 - 6) (Sunday to Saturday)
  │ │ │ │ │
  * * * * *
```

---

## 2. Production CronJob Manifest

```yaml
apiVersion: batch/v1
kind: CronJob
metadata:
  name: nightly-db-backup
spec:
  schedule: "0 2 * * *"         # Daily at 2:00 AM
  timeZone: "America/New_York"   # k8s 1.27+ native timezone support!
  concurrencyPolicy: Forbid      # Never run overlapping backups!
  successfulJobsHistoryLimit: 3
  failedJobsHistoryLimit: 1
  jobTemplate:
    spec:
      activeDeadlineSeconds: 1800 # 30 min hard timeout
      template:
        spec:
          restartPolicy: OnFailure
          containers:
          - name: backup
            image: postgres:15-alpine
            command: ["pg_dump", "-h", "db-host", "-U", "admin", "prod"]
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [03 - Failure Handling](./03-Failure-Handling-BackoffLimit-and-Pod-Failure-Policy.md) | [README](./README.md) | [05 - Concurrency Policies](./05-Concurrency-Policies-Allow-Forbid-and-Replace.md) |
