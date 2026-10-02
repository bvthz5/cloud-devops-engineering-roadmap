# 01 - Kubernetes Authentication: X.509, OIDC, and Tokens

## 1. How Kubernetes Authenticates Users

Kubernetes does not have a database of "User" objects in `etcd`. Instead, user identity is verified externally through:
1. **X.509 Client Certificates:** The Common Name (`CN`) is the username, and Organization (`O`) represents groups.
2. **OpenID Connect (OIDC):** Corporate SSO integration (Okta, Azure AD, Keycloak, Google Workspace).
3. **Bearer Tokens:** ServiceAccount JWT tokens for automated processes.

```text
Request ──► kube-apiserver
              ├── Extracts X.509 Certificate: CN=alice, O=developers, O=platform
              └── Extracts Bearer Token: /var/run/secrets/kubernetes.io/serviceaccount/token
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [README](./README.md) | [README](./README.md) | [02 - RBAC Roles & Bindings](./02-Role-ClusterRole-RoleBinding-and-ClusterRoleBinding.md) |
