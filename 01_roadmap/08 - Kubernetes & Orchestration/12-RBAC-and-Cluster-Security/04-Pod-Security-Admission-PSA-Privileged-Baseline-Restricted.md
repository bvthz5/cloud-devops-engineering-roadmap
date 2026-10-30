# 04 - Pod Security Admission (PSA): Privileged, Baseline, and Restricted

## 1. What Is Pod Security Admission (PSA)?

PSA is the native replacement for deprecated PodSecurityPolicies (PSP). It enforces the **Pod Security Standards (PSS)** across three levels:

| Level | Description |
|---|---|
| **Privileged** | Unrestricted; allows host namespaces, root execution, and host path mounts. |
| **Baseline** | Minimally restrictive; prevents known privilege escalations. |
| **Restricted** | Heavily hardened; requires non-root execution, drops all capabilities, read-only root FS. |

---

## 2. Namespace Label Enforcement

Enforced natively via namespace labels:

```yaml
apiVersion: v1
kind: Namespace
metadata:
  name: production-secured
  labels:
    pod-security.kubernetes.io/enforce: restricted
    pod-security.kubernetes.io/enforce-version: latest
    pod-security.kubernetes.io/warn: baseline
    pod-security.kubernetes.io/audit: restricted
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 03 - ServiceAccounts and Bound Token Projection](./03-ServiceAccounts-and-Bound-Token-Projection.md) | [Index](../../../README.md) | [05 - Admission Controllers Mutating and Validating Webhooks →](./05-Admission-Controllers-Mutating-and-Validating-Webhooks.md) |
