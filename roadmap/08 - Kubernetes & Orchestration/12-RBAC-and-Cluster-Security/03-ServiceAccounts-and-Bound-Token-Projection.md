# 03 - ServiceAccounts and Bound Token Projection

## 1. The Security Risk of Legacy Tokens

In Kubernetes < 1.22, ServiceAccount tokens were static, non-expiring secrets stored permanently in `etcd`. If stolen, an attacker had permanent cluster access.

---

## 2. Bound ServiceAccount Token Projection (k8s 1.22+)

Modern Kubernetes projects short-lived JWT tokens directly into pods:
- Bound to the **specific Pod instance** (token is invalidated immediately if the pod is deleted).
- Bound to a **specific Audience** (e.g. `https://vault.internal`).
- Enforces an **explicit Expiration / TTL** (e.g. 1 hour) with automatic rotation by kubelet.

```yaml
spec:
  containers:
  - name: app
    volumeMounts:
    - name: token-volume
      mountPath: /var/run/secrets/tokens
  volumes:
  - name: token-volume
    projected:
      sources:
      - serviceAccountToken:
          audience: "vault-api"
          expirationSeconds: 3600
          path: vault-token
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [02 - RBAC Roles & Bindings](./02-Role-ClusterRole-RoleBinding-and-ClusterRoleBinding.md) | [README](./README.md) | [04 - Pod Security Admission](./04-Pod-Security-Admission-PSA-Privileged-Baseline-Restricted.md) |
