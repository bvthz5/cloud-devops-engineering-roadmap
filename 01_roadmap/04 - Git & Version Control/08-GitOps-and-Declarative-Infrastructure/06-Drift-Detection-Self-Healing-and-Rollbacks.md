# 06 - Drift Detection, Self-Healing, and Rollbacks

## 1. Drift Detection and Automated Self-Healing

- **Configuration Drift:** When someone runs `kubectl edit deployment` directly in production to scale replicas or change environment variables.
- **GitOps Self-Healing:** The Argo CD / Flux controller detects that live state does not match Git desired state, and **overwrites the manual change within seconds**, returning the cluster to the Git state of truth.

---

## 2. Instant Rollbacks via Git Revert

In traditional pipelines, rolling back requires re-running complex deployment scripts.
In GitOps, **rolling back is simply a Git commit**:
```bash
git revert HEAD
git push origin main
```
The controller detects the revert commit and automatically scales the cluster back to the previous stable release within 5 seconds.

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 05 - Environment Promotion Strategies Kustomize and Helm](./05-Environment-Promotion-Strategies-Kustomize-and-Helm.md) | [Index](../../../README.md) | [07 - Real World Scenarios →](./07-Real-World-Scenarios.md) |
