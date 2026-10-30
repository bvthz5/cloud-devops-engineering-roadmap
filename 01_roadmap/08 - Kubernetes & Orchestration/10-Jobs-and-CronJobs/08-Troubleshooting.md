# 08 - Troubleshooting & Diagnostic Runbooks

## Diagnostic Decision Tree: CronJob Not Triggering

```text
[ Symptom: CronJob is not spawning any Jobs ]
                         │
                         ▼
        Check CronJob status: kubectl get cronjob <name>
        Look at "LAST SCHEDULE" column:
        ├── <none> or overdue ──► Check kube-controller-manager logs:
        │                         kubectl logs -n kube-system -l k8s-app=kube-controller-manager
        │                         Look for timezone errors or clock skew.
        └── SCHEDULED, but no job created:
                         │
                         ▼
        Is concurrencyPolicy: Forbid active?
        kubectl get jobs -l job-name
        If a previous Job is still "Running" or stuck, Forbid will prevent new jobs!
        Terminate the stuck Job to unlock execution.
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 07 - Real World Scenarios](./07-Real-World-Scenarios.md) | [Index](../../../README.md) | [09 - Interview QA →](./09-Interview-QA.md) |
