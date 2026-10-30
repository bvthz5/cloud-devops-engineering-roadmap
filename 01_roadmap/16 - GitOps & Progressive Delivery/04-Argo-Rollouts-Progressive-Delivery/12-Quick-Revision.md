# Quick Revision Notes - Argo Rollouts & Progressive Delivery

> **Module**: Argo Rollouts & Progressive Delivery

---

## ⚡ Key Cheat Sheet

| GitOps Tool / Concept | Core Function | Key Advantage |
|---|---|---|
| **ArgoCD** | Declarative GitOps CD engine | Rich UI, ApplicationSets, Sync Waves |
| **FluxCD v2** | Modular CNCF GitOps Toolkit | Native Helm/Kustomize controllers, OCI support |
| **Argo Rollouts** | Progressive delivery controller | Metric-driven Canary & Blue-Green rollouts |
| **Kustomize** | Template-free manifest patcher | Built into `kubectl -k`, no template syntax |
| **OpenGitOps** | CNCF GitOps Standard | 4 Core Principles: Declarative, Versioned, Pulled, Reconciled |

---

## 📝 Top 5 Rules to Remember
1. **Git is the single source of truth**; never modify production clusters manually with `kubectl edit`.
2. **Enable `selfHeal: true` and `prune: true`** in ArgoCD Application sync policies.
3. **Separate application source code from Kubernetes environment manifest repos**.
4. **Use Sync Waves to control resource creation ordering** (CRDs -> Storage -> Apps).
5. **Automate Canary rollbacks with Argo Rollouts + Prometheus** to limit production outage impact.

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 11 - MCQ](./11-MCQ.md) | [Index](../../../README.md) | [Next Module (05-Kustomize-vs-Helm-for-GitOps) →](../05-Kustomize-vs-Helm-for-GitOps/01-Kustomize-Architecture-Bases-Overlays-and-Patches.md) |
