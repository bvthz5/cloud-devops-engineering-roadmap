# Module 08: GitOps and Declarative Infrastructure Control

Welcome to **Module 08: GitOps and Declarative Infrastructure Control**. GitOps elevates Git from a code repository into the single source of truth for entire cloud infrastructures, Kubernetes clusters, and security policies.

---

## 🎯 Learning Objectives

By the end of this module, you will be able to:
1. Explain the 4 OpenGitOps principles and why **Pull-Based Reconciliation** outperforms legacy CI Push pipelines.
2. Architect enterprise GitOps workflows using **Argo CD** and **Flux CD**.
3. Structure multi-environment repositories using **Monorepo vs. Polyrepo** patterns.
4. Solve the GitOps secret paradox using **Mozilla SOPS**, **Bitnami Sealed Secrets**, and **External Secrets Operator (ESO)**.
5. Manage multi-cluster continuous deployments using **Kustomize Overlays** and **Helm Values**.
6. Implement automated drift detection, self-healing, and rollback mechanisms.

---

## 📚 Module Index

| # | Topic | Description |
|---|---|---|
| 01 | [GitOps Core Principles & Pull Model](./01-GitOps-Core-Principles-and-Pull-vs-Push.md) | The 4 OpenGitOps principles, in-cluster controllers vs CI runners |
| 02 | [Argo CD & Flux CD Architecture](./02-Argo-CD-and-Flux-CD-Architecture.md) | Reconciliation loops, Custom Resource Definitions (CRDs), sync waves |
| 03 | [Repository Topologies: Mono vs Poly](./03-Repository-Topologies-Monorepo-vs-Polyrepo.md) | App code vs infrastructure manifest repos, permission boundaries |
| 04 | [Secret Management in GitOps](./04-Secret-Management-in-GitOps-SOPS-and-Vault.md) | Never commit raw secrets; Sealed Secrets, SOPS with KMS, External Secrets |
| 05 | [Environment Promotion Strategies](./05-Environment-Promotion-Strategies-Kustomize-and-Helm.md) | Dev -> Staging -> Prod promotions using Git branches vs directories/tags |
| 06 | [Drift Detection & Self-Healing](./06-Drift-Detection-Self-Healing-and-Rollbacks.md) | Out-of-band change detection, automated cluster reconciliation, instant git reverts |
| 07 | [Real-World Production Scenarios](./07-Real-World-Scenarios.md) | Manual kubectl edit causing GitOps self-heal crash, leaked secret in git history |
| 08 | [Troubleshooting Guide](./08-Troubleshooting.md) | Argo CD out-of-sync loops, CRD schema validation failures, SOPS decryption errors |
| 09 | [Interview Questions & Answers](./09-Interview-QA.md) | 10 high-frequency production SRE/DevOps interview scenarios |
| 10 | [Hands-On Lab Practice](./10-Hands-On-Practice.md) | Encrypting Kubernetes secrets with Mozilla SOPS and defining an Argo CD Application |
| 11 | [Self-Assessment MCQs](./11-MCQ.md) | 10 scenario-based multiple-choice questions with deep explanations |
| 12 | [Quick Revision Cheat Sheet](./12-Quick-Revision.md) | GitOps architectural comparison, SOPS command sheet, Argo sync options |

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [07 - Resolving Conflicts](../07-Resolving-Conflicts-and-Troubleshooting/README.md) | [README](./README.md) | [01 - GitOps Principles](./01-GitOps-Core-Principles-and-Pull-vs-Push.md) |
