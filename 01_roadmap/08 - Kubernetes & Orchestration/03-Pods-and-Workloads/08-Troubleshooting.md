# 08 - Troubleshooting & Diagnostic Runbooks

## Diagnostic Decision Tree: Pod Startup & Crash States

```text
[ Symptom: Pod is not in "Running" state ]
                   │
                   ▼
        What is the status from 'kubectl get pods'?
        ├── CrashLoopBackOff ──► Container started, exited with non-zero code, and is restarting.
        │                        Run: kubectl logs <pod> --previous
        ├── OOMKilled (137)  ──► Container exceeded memory limit or host ran out of memory.
        │                        Run: kubectl describe pod <pod> | grep -i oom
        ├── ImagePullBackOff ──► Cannot download image (bad tag, registry auth, network timeout).
        │                        Run: kubectl describe pod <pod> (look at Events)
        └── Pending          ──► Cannot schedule to any node.
                                 Run: kubectl describe pod <pod> (check Predicate failures)
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 07 - Real World Scenarios](./07-Real-World-Scenarios.md) | [Index](../../../README.md) | [09 - Interview QA →](./09-Interview-QA.md) |
