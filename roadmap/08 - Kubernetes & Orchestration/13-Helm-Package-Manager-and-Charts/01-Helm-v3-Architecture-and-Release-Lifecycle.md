# 01 - Helm v3 Architecture and Release Lifecycle

## 1. Why Helm v3 Dropped Tiller

In Helm v2, an in-cluster pod named **Tiller** ran with `cluster-admin` privileges, acting as a massive security vulnerability.
**Helm v3 is entirely client-side (Tillerless):**
- Uses your local `~/.kube/config` permissions directly.
- Stores release state as encrypted/base64 **Kubernetes Secrets** inside the target release namespace:
  `sh.helm.release.v1.<release-name>.v1`.
- Employs a **Three-Way Strategic Merge Patch** on upgrades, respecting changes made by other controllers.

```text
[ Developer: helm upgrade my-app ./chart ]
                     │
                     ▼ (Direct HTTPS to API server using user RBAC)
         [ Kubernetes kube-apiserver ]
                     │
                     ▼
  [ Reads Release Secret: sh.helm.release.v1.my-app.v1 ]
  [ Calculates 3-Way Merge Patch ]
  [ Applies Manifest Changes ──► Creates Release Secret v2 ]
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← Prev Module (12-RBAC-and-Cluster-Security)](../12-RBAC-and-Cluster-Security/12-Quick-Revision.md) | [Index](../../../README.md) | [02 - Helm Chart Directory Structure and Chart yaml →](./02-Helm-Chart-Directory-Structure-and-Chart-yaml.md) |
