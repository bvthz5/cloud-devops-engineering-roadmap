# 09 - Interview Questions & Architectural Scenarios

### Q1: How do you verify what permissions a specific user or ServiceAccount has?
**Answer:**
Use the `kubectl auth can-i` command:
```bash
# Check if user bob can delete deployments in production
kubectl auth can-i delete deployments -n production --as=bob

# Check if a ServiceAccount can read secrets
kubectl auth can-i get secrets --as=system:serviceaccount:default:my-app-sa
```

---

### Q2: What is the difference between a RoleBinding and a ClusterRoleBinding?
**Answer:**
- A **RoleBinding** grants permissions within a single, specific namespace. Even if it references a `ClusterRole` (like the built-in `edit` or `view` roles), the permissions only apply inside that namespace.
- A **ClusterRoleBinding** grants permissions cluster-wide across all namespaces and cluster-scoped resources (like `nodes`, `persistentvolumes`, and `namespaces`).

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [08 - Troubleshooting](./08-Troubleshooting.md) | [README](./README.md) | [10 - Hands-On Practice](./10-Hands-On-Practice.md) |
