# 03 - Rollbacks, Revision History, and Change-Cause

## 1. Managing Rollout Revisions

Kubernetes tracks previous revisions of a Deployment by keeping non-zero-replica ReplicaSets around (scaled to 0).

```bash
# 1. View rollout status in real time
kubectl rollout status deployment/web-api

# 2. Check complete rollout revision history
kubectl rollout history deployment/web-api

# 3. Inspect a specific revision
kubectl rollout history deployment/web-api --revision=2

# 4. Instant Rollback to immediately preceding revision
kubectl rollout undo deployment/web-api

# 5. Rollback to a specific historical revision
kubectl rollout undo deployment/web-api --to-revision=1
```

---

## 2. The `revisionHistoryLimit` Setting

By default, Kubernetes preserves the last 10 ReplicaSets. In large clusters with frequent CI/CD builds, this bloats `etcd` memory:

```yaml
spec:
  revisionHistoryLimit: 3       # Recommended: keeps 3 historical ReplicaSets
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 02 - Rolling Update Strategy MaxSurge and MaxUnavailable](./02-Rolling-Update-Strategy-MaxSurge-and-MaxUnavailable.md) | [Index](../../../README.md) | [04 - Blue Green and Canary Deployments Native Patterns →](./04-Blue-Green-and-Canary-Deployments-Native-Patterns.md) |
