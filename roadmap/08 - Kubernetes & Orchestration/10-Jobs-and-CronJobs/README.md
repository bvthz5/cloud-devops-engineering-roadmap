# 10 - Jobs and CronJobs

> **Learning Methodology:**
> **Understand → See → Practice → Troubleshoot → Interview → Revise**

## 📌 Module Syllabus
1. `01-Kubernetes-Job-Controller-and-Completion-Guarantees.md` — Run-to-completion batch workloads: `completions`, `parallelism`, and completion guarantees.
2. `02-Parallel-Jobs-and-Work-Queue-Processing.md` — Scalable batch processing: RabbitMQ / SQS queue workers and static vs dynamic job partitioning.
3. `03-Failure-Handling-BackoffLimit-and-Pod-Failure-Policy.md` — Failure resilience: `backoffLimit`, retry counts, non-retryable exit codes, and `podFailurePolicy`.
4. `04-CronJob-Architecture-and-Schedule-Syntax.md` — Periodic schedules: standard 5-part cron syntax, timezones (`spec.timeZone`), and the CronJob controller loop.
5. `05-Concurrency-Policies-Allow-Forbid-and-Replace.md` — Overlapping execution defense: `concurrencyPolicy` (`Allow`, `Forbid`, `Replace`) and deadlock prevention.
6. `06-History-Limits-CleanUp-and-Deadlines.md` — Cluster hygiene: `successfulJobsHistoryLimit`, `failedJobsHistoryLimit`, `ttlSecondsAfterFinished`, and `activeDeadlineSeconds`.
7. `07-Real-World-Scenarios.md` — Production post-mortems: concurrent DB migration deadlocks, and CronJob pileup exhausting node memory.
8. `08-Troubleshooting.md` — Diagnostic runbook for stuck CronJobs, skipped executions, and infinite restart loops.
9. `09-Interview-QA.md` — 10 Senior SRE/DevOps interview scenarios on batch processing in Kubernetes.
10. `10-Hands-On-Practice.md` — Production lab: deploying a self-cleaning parallel batch processing Job with retries and failure policies.
11. `11-MCQ.md` — 10 scenario-based multiple choice questions with detailed explanations.
12. `12-Quick-Revision.md` — High-density Job and CronJob reference cheat sheet.

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [Module 09 - StatefulSets](../09-StatefulSets-and-DaemonSets/README.md) | [README](./README.md) | [01 - Job Controller](./01-Kubernetes-Job-Controller-and-Completion-Guarantees.md) |
