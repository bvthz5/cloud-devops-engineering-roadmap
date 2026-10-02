# 06 - Reloader: Automatic Pod Rollout on Config Changes

## 1. The Environment Variable Stale State Problem

- When a ConfigMap is mounted as a **Volume**, the kubelet eventually updates the file inside the container (~60 seconds).
- However, when a ConfigMap is injected as an **Environment Variable (`env`)**, the container process **NEVER** receives the update until the Pod is deleted and recreated!

---

## 2. Automated Rolling Restarts with Stakater Reloader

**Reloader** is an open-source Kubernetes controller that watches ConfigMaps and Secrets. When a change occurs, it automatically triggers a rolling upgrade on associated Deployments:

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: api-service
  annotations:
    # Automatically restart this deployment when app-config or app-secret changes!
    reloader.stakater.com/auto: "true"
spec:
  replicas: 3
  template:
    ...
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [05 - External Secrets Operator](./05-External-Secrets-Operator-ESO-and-Vault-Sync.md) | [README](./README.md) | [07 - Real-World Scenarios](./07-Real-World-Scenarios.md) |
