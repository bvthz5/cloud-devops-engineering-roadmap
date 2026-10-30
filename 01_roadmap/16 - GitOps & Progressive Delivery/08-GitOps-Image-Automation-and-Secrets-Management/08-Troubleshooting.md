# Troubleshooting Guide - GitOps Image Automation & Secrets Management

> **Module**: GitOps Image Automation & Secrets Management

---

## 🔍 Common Issue 1: External Secrets Operator (ESO) Sync Failure (`SecretNotFound`)

### Symptom
ExternalSecret resource shows status `SecretSynced=False` with error `SecretStore aws-parameter-store err: AccessDenied or Secret /production/db/password not found`.

### Root Cause & Resolution
1. **Missing IAM Permissions**: The ServiceAccount used by ESO lacks `ssm:GetParameter` or `secretsmanager:GetSecretValue` on AWS.
2. **Workload Identity Annotation**: Verify the `ClusterSecretStore` references a service account annotated for AWS IAM OIDC / GCP Workload Identity.
3. **Path Mismatch**: Verify exact key path name in AWS Secrets Manager or HashiCorp Vault.

---

## 🔍 Common Issue 2: ArgoCD ApplicationSet Matrix Generator Reconciliation Timeout

### Symptom
ApplicationSet controller fails to generate target Applications across 50 clusters, returning `error expanding matrix generator: git repo connection timeout`.

### Root Cause & Resolution
1. **Git Polling Rate Limits**: Polling 50 target directories in a single repository triggers GitHub API rate limits.
2. **Enable Git Webhooks**: Configure GitHub/GitLab webhooks to notify ArgoCD on push events, avoiding high-frequency polling.
3. **Increase Controller Replicas**: Scale ArgoCD ApplicationSet controller replicas and increase `--app-resync` intervals.

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 07 - Real World Scenarios](./07-Real-World-Scenarios.md) | [Index](../../../README.md) | [09 - Interview QA →](./09-Interview-QA.md) |
