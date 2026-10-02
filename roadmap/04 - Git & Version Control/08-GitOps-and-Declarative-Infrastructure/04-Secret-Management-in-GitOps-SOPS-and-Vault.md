# 04 - Secret Management in GitOps: SOPS and Vault

## 1. The GitOps Secret Paradox

GitOps mandates that **all** configurations live in Git. However, committing raw Kubernetes Secret manifests exposes plaintext base64 credentials to anyone with read access!

---

## 2. Production Secret Solutions

### 1. Mozilla SOPS (Secrets OPerationS)
Encrypts only the *values* of YAML files using AWS KMS, GCP KMS, Azure Key Vault, or age keys, leaving the *keys* human-readable for diffing:
```yaml
apiVersion: v1
kind: Secret
metadata:
  name: db-credentials
data:
  password: ENC[AES256_GCM,data:xyz123...] # Encrypted!
```

### 2. External Secrets Operator (ESO)
Kubernetes in-cluster operator that reads secrets directly from **AWS Secrets Manager**, **Google Secret Manager**, or **HashiCorp Vault** and dynamically creates local Kubernetes Secrets in memory. Git stores only the pointer reference!

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [03 - Repo Topologies](./03-Repository-Topologies-Monorepo-vs-Polyrepo.md) | [README](./README.md) | [05 - Environment Promotion](./05-Environment-Promotion-Strategies-Kustomize-and-Helm.md) |
