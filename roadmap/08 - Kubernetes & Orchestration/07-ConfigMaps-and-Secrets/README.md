# 07 - ConfigMaps and Secrets

> **Learning Methodology:**
> **Understand → See → Practice → Troubleshoot → Interview → Revise**

## 📌 Module Syllabus
1. `01-ConfigMap-Creation-Environment-and-Volume-Projections.md` — Decoupling configuration from containers: literal values, files, directories, and volume mounts.
2. `02-Kubernetes-Secrets-Types-and-Base64-Encoding-Realities.md` — Secret types (`Opaque`, `dockerconfigjson`, `tls`), base64 misconceptions, and security implications.
3. `03-Secret-Encryption-at-Rest-with-KMS-Providers.md` — Hardening etcd: `EncryptionConfiguration`, AES-GCM, and AWS KMS / GCP Cloud KMS / Azure Key Vault plugins.
4. `04-Immutable-ConfigMaps-and-Secrets-for-Scale.md` — Reducing kube-apiserver watch overhead with `immutable: true` and protection against accidental drift.
5. `05-External-Secrets-Operator-ESO-and-Vault-Sync.md` — Production GitOps secret sync: ESO, `SecretStore`, `ExternalSecret`, and HashiCorp Vault integration.
6. `06-Reloader-Automatic-Pod-Rollout-on-Config-Changes.md` — Overcoming the static environment variable limitation: Stakater Reloader and automated rolling restarts.
7. `07-Real-World-Scenarios.md` — Production post-mortems: committed plain-text secrets in git, and truncated config volume crashes.
8. `08-Troubleshooting.md` — Diagnostic runbook for Secret/ConfigMap mount permissions, symlink updates, and missing keys.
9. `09-Interview-QA.md` — 10 Senior SRE/DevOps interview scenarios on Kubernetes configuration and secret management.
10. `10-Hands-On-Practice.md` — Production lab: deploying External Secrets Operator syncing secrets from an external store into live pods.
11. `11-MCQ.md` — 10 scenario-based multiple choice questions with collapsible explanations.
12. `12-Quick-Revision.md` — High-density ConfigMap and Secret reference cheat sheet.

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [Module 06 - Ingress](../06-Ingress-Controllers-and-Routing/README.md) | [README](./README.md) | [01 - ConfigMap Creation](./01-ConfigMap-Creation-Environment-and-Volume-Projections.md) |
