# 05 - External Secrets Operator (ESO) and Vault Sync

## 1. Why GitOps Needs External Secrets Operator

In modern GitOps (ArgoCD, Flux), all application manifests are committed to Git. Committing plain Kubernetes Secrets to Git violates security compliance.
**External Secrets Operator (ESO)** bridges external secret managers (HashiCorp Vault, AWS Secrets Manager, GCP Secret Manager) into Kubernetes.

```text
+----------------------------+
| External Secret Store      |
| (AWS Secrets Mgr / Vault)  |
+-------------+--------------+
              | Polled securely via IAM Role / ServiceAccount
              v
+----------------------------+
| External Secrets Operator  |
| - SecretStore CRD          |
| - ExternalSecret CRD       |
+-------------+--------------+
              | Synchronizes into native k8s resource
              v
+----------------------------+
| Native Kubernetes Secret   |
| (Consumed by your Pods!)   |
+----------------------------+
```

---

## 2. Production ExternalSecret Manifest

```yaml
apiVersion: external-secrets.io/v1beta1
kind: ExternalSecret
metadata:
  name: database-credentials
spec:
  refreshInterval: "1h"
  secretStoreRef:
    name: vault-backend
    kind: ClusterSecretStore
  target:
    name: db-secret-k8s
    creationPolicy: Owner
  data:
  - secretKey: DB_PASSWORD
    remoteRef:
      key: secret/data/prod/db
      property: password
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 04 - Immutable ConfigMaps and Secrets for Scale](./04-Immutable-ConfigMaps-and-Secrets-for-Scale.md) | [Index](../../../README.md) | [06 - Reloader Automatic Pod Rollout on Config Changes →](./06-Reloader-Automatic-Pod-Rollout-on-Config-Changes.md) |
