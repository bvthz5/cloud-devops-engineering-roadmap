# 08 - Troubleshooting & Diagnostic Runbooks

## Diagnostic Decision Tree: Deployment Rollout Stuck

```text
[ Symptom: "Waiting for deployment "api" rollout to finish: 1 out of 3 new replicas have been updated..." ]
                                  │
                                  ▼
        Check ReplicaSets: kubectl get rs -l app=api
        Identify the new ReplicaSet (look for highest revision / current hash)
                                  │
                                  ▼
        Are pods crashing or failing to start in the new ReplicaSet?
        kubectl get pods -l pod-template-hash=<new-hash>
        ├── ImagePullBackOff ──► Wrong image tag or private registry credentials missing.
        ├── CrashLoopBackOff ──► Missing environment variables, secrets, or startup crash.
        └── Pending          ──► Insufficient CPU/Memory on cluster nodes.
                                 Run: kubectl describe pod <new-pod>
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [07 - Real-World Scenarios](./07-Real-World-Scenarios.md) | [README](./README.md) | [09 - Interview QA](./09-Interview-QA.md) |
