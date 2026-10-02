# 02 - Role, ClusterRole, RoleBinding, and ClusterRoleBinding

## 1. Scope Matrix

| Resource | Scope | What It Can Control |
|---|---|---|
| **Role** | Namespaced | Resources within a single specific namespace (e.g. `pods` in `payments`). |
| **ClusterRole** | Cluster-wide | Cluster-scoped resources (`nodes`, `namespaces`, `PVs`) OR across all namespaces. |
| **RoleBinding** | Namespaced | Binds a Role (or ClusterRole) to users/groups within a specific namespace. |
| **ClusterRoleBinding** | Cluster-wide | Binds a ClusterRole to users/groups across the entire cluster. |

---

## 2. Production Least-Privilege Role Manifest

```yaml
apiVersion: rbac.authorization.k8s.io/v1
kind: Role
metadata:
  namespace: billing
  name: deployment-operator
rules:
- apiGroups: ["apps"]
  resources: ["deployments"]
  verbs: ["get", "list", "watch", "patch", "update"]
- apiGroups: [""]
  resources: ["pods", "pods/log"]
  verbs: ["get", "list"]
---
apiVersion: rbac.authorization.k8s.io/v1
kind: RoleBinding
metadata:
  namespace: billing
  name: bind-deployment-operator
subjects:
- kind: Group
  name: "billing-devs"
  apiGroup: rbac.authorization.k8s.io
roleRef:
  kind: Role
  name: deployment-operator
  apiGroup: rbac.authorization.k8s.io
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 01 - Kubernetes Authentication X509 OIDC and Tokens](./01-Kubernetes-Authentication-X509-OIDC-and-Tokens.md) | [Index](../../../README.md) | [03 - ServiceAccounts and Bound Token Projection →](./03-ServiceAccounts-and-Bound-Token-Projection.md) |
