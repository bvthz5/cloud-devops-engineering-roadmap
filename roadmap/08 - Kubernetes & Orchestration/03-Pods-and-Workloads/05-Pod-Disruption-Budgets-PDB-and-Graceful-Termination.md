# 05 - Pod Disruption Budgets (PDB) and Graceful Termination

## 1. Graceful Termination Timeline

When a Pod is deleted or a node is drained, Kubernetes executes a graceful shutdown sequence:

```text
API Server marks Pod "Terminating"
   ├── 1. Removed from Service EndpointSlices (Stops receiving new traffic)
   ├── 2. Executes 'preStop' hook (if defined)
   ├── 3. Sends SIGTERM to container main process (PID 1)
   └── 4. Waits up to 'terminationGracePeriodSeconds' (default 30s)
          ├── If process exits ──► Pod cleaned up immediately
          └── If timeout expires ──► Kernel sends SIGKILL (Immediate abort)
```

---

## 2. Pod Disruption Budgets (PDB)

A **Pod Disruption Budget (PDB)** limits the number of pods of a replicated application that can be simultaneously down from voluntary disruptions (e.g., node drains during Kubernetes version upgrades):

```yaml
apiVersion: policy/v1
kind: PodDisruptionBudget
metadata:
  name: payments-pdb
spec:
  minAvailable: 2             # At least 2 pods must remain online
  selector:
    matchLabels:
      app: payments
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [04 - QoS & Resource Limits](./04-Resource-Requests-Limits-and-Quality-of-Service-QoS.md) | [README](./README.md) | [06 - Security Contexts](./06-Security-Contexts-RunAsUser-and-Privilege-Escalation.md) |
