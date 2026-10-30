# 09 - Interview Questions & Architectural Scenarios

### Q1: What is the difference between `restartPolicy: OnFailure` and `restartPolicy: Never` in a Job?
**Answer:**
- `OnFailure`: When the container fails (non-zero exit code), Kubelet restarts the container **inside the same Pod** on the same node without creating a new Pod.
- `Never`: When the container fails, Kubelet terminates the Pod. The Job Controller is responsible for creating a **brand new Pod** (potentially on a completely different worker node) to attempt the retry.

---

### Q2: What happens if a CronJob schedule is missed due to cluster downtime?
**Answer:**
If the time since the last schedule exceeds `spec.startingDeadlineSeconds` (e.g. cluster was down for 2 hours and deadline is 100 seconds), the CronJob controller counts this as a missed execution and **does not start the job**. If `startingDeadlineSeconds` is unset and more than 100 schedules were missed, the CronJob controller stops scheduling jobs entirely until manually reset.

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 08 - Troubleshooting](./08-Troubleshooting.md) | [Index](../../../README.md) | [10 - Hands On Practice →](./10-Hands-On-Practice.md) |
