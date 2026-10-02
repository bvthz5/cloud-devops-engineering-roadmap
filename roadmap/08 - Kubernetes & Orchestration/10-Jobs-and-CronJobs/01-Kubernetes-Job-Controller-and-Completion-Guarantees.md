# 01 - Kubernetes Job Controller and Completion Guarantees

## 1. What Is a Kubernetes Job?

While Deployments run long-lived, continuous processes (web servers, APIs), a **Job** runs pods until a specified number of them successfully terminate (exit code 0).

```text
Job Specification (completions: 3, parallelism: 1)
   │
   ├── Pod 1 starts ──► Runs script ──► Exits 0 (Success)
   ├── Pod 2 starts ──► Runs script ──► Exits 0 (Success)
   └── Pod 3 starts ──► Runs script ──► Exits 0 (Success)
   │
   ▼
Job Status: Complete (Job Controller stops spawning pods)
```

---

## 2. Job Specification Manifest

```yaml
apiVersion: batch/v1
kind: Job
metadata:
  name: data-indexer
spec:
  completions: 5                # Total successful pod completions required
  parallelism: 2                # Up to 2 pods running concurrently
  backoffLimit: 3               # Max retries before marking Job as failed
  ttlSecondsAfterFinished: 300  # Automatically delete Job 5 mins after finish!
  template:
    spec:
      restartPolicy: OnFailure  # Must be OnFailure or Never (Never Always!)
      containers:
      - name: indexer
        image: python:3.11-alpine
        command: ["python", "-c", "import time; print('Processing batch'); time.sleep(5)"]
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [README](./README.md) | [README](./README.md) | [02 - Parallel Jobs](./02-Parallel-Jobs-and-Work-Queue-Processing.md) |
