# 02 - Rolling Update Strategy: MaxSurge and MaxUnavailable

## 1. Zero-Downtime Math

When you update `spec.template.spec.containers[0].image`, the Deployment Controller creates a **new ReplicaSet** and gradually scales up the new while scaling down the old.

```text
Rolling Update Mechanics (Replicas: 4, maxSurge: 25%, maxUnavailable: 25%):
Target Replicas: 4
maxSurge (25% of 4 = 1): Max pods allowed during update = 4 + 1 = 5
maxUnavailable (25% of 4 = 1): Min pods available during update = 4 - 1 = 3

Step 0: Old RS = 4, New RS = 0 (Total: 4)
Step 1: Old RS = 4, New RS = 1 (Total: 5 -> MaxSurge reached)
Step 2: New Pod becomes "Ready" -> Old RS scaled to 3 (Total: 4)
Step 3: New RS scaled to 2 (Total: 5)
...
Final:  Old RS = 0, New RS = 4 (Total: 4)
```

---

## 2. Configuration Matrix

```yaml
spec:
  replicas: 10
  strategy:
    type: RollingUpdate
    rollingUpdate:
      maxSurge: 25%             # Allows up to 13 pods during rollout
      maxUnavailable: 0         # STRICT: Never allow less than 10 healthy pods!
```

| Strategy Goal | `maxSurge` | `maxUnavailable` | Tradeoff |
|---|---|---|---|
| **Zero Downtime (Standard)** | `25%` | `25%` | Balanced speed and capacity. |
| **Strict SLA (Mission-Critical)**| `25%` | `0` | Requires extra cluster compute capacity. |
| **Resource-Constrained Edge** | `0` | `1` | Saves RAM/CPU, but temporarily reduces capacity. |
| **Recreate** | N/A | N/A | Total downtime: terminates all old pods before starting new. |

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [01 - Deployment Controller](./01-Deployment-Controller-and-ReplicaSet-Reconciliation.md) | [README](./README.md) | [03 - Rollbacks & Revision History](./03-Rollbacks-Revision-History-and-Change-Cause.md) |
