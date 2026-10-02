# 03 - Failure Handling: BackoffLimit and Pod Failure Policy

## 1. Retries with `backoffLimit`

When a Job container exits with a non-zero exit code:
- The Job controller recreates the Pod with an exponential delay: `10s`, `20s`, `40s`, etc.
- If failures exceed `spec.backoffLimit` (default: 6), the entire Job transitions to `Failed`.

---

## 2. Granular Retries with `podFailurePolicy`

Starting in Kubernetes 1.25+, `podFailurePolicy` allows differentiating between retryable transient errors (network glitch) and non-retryable fatal bugs (syntax error, bad SQL query):

```yaml
spec:
  podFailurePolicy:
    rules:
    - action: FailJob           # Do NOT retry; fail immediately!
      onExitCodes:
        containerName: worker
        operator: In
        values: [42, 127]       # Exit code 127 = binary not found
    - action: Ignore            # Don't count against backoffLimit
      onPodConditions:
      - type: DisruptionTarget  # Pod killed due to node spot preemption
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [02 - Parallel Jobs](./02-Parallel-Jobs-and-Work-Queue-Processing.md) | [README](./README.md) | [04 - CronJob Architecture](./04-CronJob-Architecture-and-Schedule-Syntax.md) |
