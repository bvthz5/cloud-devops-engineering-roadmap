# Interview Q&A - FluxCD GitOps Toolkit

> **Module**: FluxCD GitOps Toolkit

---

### Q1: What are the 4 core principles of OpenGitOps?
**Answer**:
1. **Declarative**: The target system must be described declaratively.
2. **Versioned & Immutable**: The desired state is stored in a version-controlled store (Git) ensuring auditability.
3. **Pulled Automatically**: Software agents pull the desired state from the store automatically.
4. **Continuously Reconciled**: Agents continuously observe live state and reconcile any drift back to the desired state.

---

### Q2: How do Sync Waves work in ArgoCD?
**Answer**:
Sync Waves control the exact phase and order in which ArgoCD applies Kubernetes resources. Resources are assigned an annotation (`argocd.argoproj.io/sync-wave: "1"`). ArgoCD applies wave `-1` first (e.g., Namespaces, CRDs), waits for them to become Healthy, then proceeds to wave `0` (Services, PVCs), and finally wave `1` (Deployments, Rollouts).

---

### Q3: What is the difference between Kustomize and Helm in a GitOps workflow?
**Answer**:
- **Helm**: Parameter-driven templating engine (`values.yaml` + Jinja-like templates) ideal for packaging and distributing third-party off-the-shelf software (e.g., Redis, NGINX).
- **Kustomize**: Template-free overlay engine that uses base manifests and environment patches (`kustomization.yaml`), ideal for managing first-party internal microservice variations across Dev, Staging, and Prod.

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 08 - Troubleshooting](./08-Troubleshooting.md) | [Index](../../../README.md) | [10 - Hands On Practice →](./10-Hands-On-Practice.md) |
