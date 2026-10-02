# 12 - Quick-Revision & Enterprise Cheat Sheet

## RBAC & Security Summary

- **Authentication (AuthN):** X.509 certs, OIDC tokens, ServiceAccount JWTs.
- **Authorization (AuthZ):** Roles (namespaced) vs ClusterRoles (cluster-wide).
- **Audit Tool:** `kubectl auth can-i <verb> <resource> --as=<user>`.
- **Pod Security Standards (PSS):** Privileged ➔ Baseline ➔ Restricted.
- **Policy as Code:** Kyverno (native YAML) / OPA Gatekeeper (Rego).

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 11 - MCQ](./11-MCQ.md) | [Index](../../../README.md) | [Next Module (13-Helm-Package-Manager-and-Charts) →](../13-Helm-Package-Manager-and-Charts/01-Helm-v3-Architecture-and-Release-Lifecycle.md) |
