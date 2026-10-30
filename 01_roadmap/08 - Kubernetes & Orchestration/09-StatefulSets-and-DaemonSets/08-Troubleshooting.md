# 08 - Troubleshooting & Diagnostic Runbooks

## Diagnostic Decision Tree: StatefulSet Stuck in Rollout

```text
[ Symptom: "Waiting for 1 pods to be ready..." (StatefulSet update frozen) ]
                                  │
                                  ▼
        Which ordinal pod is not in Ready state?
        kubectl get pods -l app=my-statefulset
        (Remember: OrderedReady will FREEZE if pod N is not healthy!)
                                  │
                                  ▼
        Check logs and events of the failing ordinal pod:
        kubectl describe pod <failing-ordinal-pod>
        ├── Liveness/Readiness probe failing ──► App startup issue or DB schema lock.
        └── FailedMount / Volume In Use      ──► Cloud EBS volume still attached to dead VM.
                                                 Force detach or drain old node.
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 07 - Real World Scenarios](./07-Real-World-Scenarios.md) | [Index](../../../README.md) | [09 - Interview QA →](./09-Interview-QA.md) |
