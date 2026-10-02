# Quick Revision Notes - Flagger Progressive Delivery & Service Mesh

> **Module**: Flagger Progressive Delivery & Service Mesh

---

## ⚡ Key Cheat Sheet

| Feature / Tool | Primary Purpose | Key Architectural Highlight |
|---|---|---|
| **ApplicationSets** | Multi-cluster GitOps orchestration | Matrix/Git/Cluster generators |
| **External Secrets Operator** | Secure secret sync from Cloud Vaults | Integrates AWS, Azure, GCP & HashiCorp Vault |
| **Flagger** | Service Mesh Canary progressive delivery | Automated Prometheus metric analysis & rollbacks |
| **Sealed Secrets / SOPS** | Git secret encryption | Asymmetric KMS/GPG encryption for Git repos |
| **Argo Notifications** | Real-time GitOps event alerting | Integrates Slack, MS Teams & PagerDuty |

---

## 📝 Top 5 Rules to Remember
1. **Never store unencrypted secret values in Git repositories**; use ESO or SOPS.
2. **Use ArgoCD ApplicationSets for scaling multi-cluster deployments** across 10+ clusters.
3. **Automate Canary releases using Flagger or Argo Rollouts** with Prometheus error rate guardrails.
4. **Require signed Git commits (GPG/SSH)** for all production deployment repositories.
5. **Configure Argo Notifications for Slack/PagerDuty** to track deployment sync failures in real time.
