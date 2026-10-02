# Blue-Green Deployments and Active/Preview Services

> **Module**: Argo Rollouts & Progressive Delivery  
> **Level**: Production & Deep-Dive  

---

## 📌 Executive Summary
Mastering **Blue-Green Deployments and Active/Preview Services** is essential for building modern cloud-native Kubernetes delivery pipelines. GitOps replaces push-based CI scripts with declarative, pull-based reconciliation loops that continuously audit and sync cluster state with Git repositories.

---

## 🏗 Architecture & GitOps Control Loop

```
+-------------------------------------------------------------------------------+
|                       GITOPS RECONCILIATION ARCHITECTURE                     |
+-------------------------------------------------------------------------------+
|  Git Repository  <-- Poll/Webhook --  GitOps Operator (ArgoCD / Flux)        |
|  (Desired State)                      (Compares & Self-Heals)                 |
|                                                |                              |
|                                                v                              |
|                                       Kubernetes Cluster                      |
|                                       (Live State)                            |
+-------------------------------------------------------------------------------+
```

### Key Pillars
1. **Declarative State**: System state is specified declaratively in Git (Kustomize/Helm manifests).
2. **Versioned & Immutable**: Desired state is version-controlled with complete Git audit history.
3. **Automated Pull Reconciliation**: Software agents continuously pull changes from Git and apply them to the cluster.
4. **Self-Healing & Drift Correction**: Manual out-of-band cluster modifications are automatically detected and overridden back to Git state.

---

## 🛠 Declarative CRD Examples

### ArgoCD Application CRD Manifest
```yaml
apiVersion: argoproj.io/v1alpha1
kind: Application
metadata:
  name: microservice-payments
  namespace: argocd
spec:
  project: default
  source:
    repoURL: 'https://github.com/enterprise/k8s-fleet.git'
    targetRevision: main
    path: apps/payments/overlays/production
  destination:
    server: 'https://kubernetes.default.svc'
    namespace: production
  syncPolicy:
    automated:
      prune: true
      selfHeal: true
    syncOptions:
      - CreateNamespace=true
```

### Flux v2 Kustomization Source Manifest
```yaml
apiVersion: kustomize.toolkit.fluxcd.io/v1
kind: Kustomization
metadata:
  name: podinfo
  namespace: flux-system
spec:
  interval: 5m
  targetNamespace: default
  sourceRef:
    kind: GitRepository
    name: podinfo
  path: ./kustomize
  prune: true
  wait: true
```

---

## 💡 Industry Best Practices
- **Separate Application Code from Manifest Repositories**: Keep application source code in app repos and Kubernetes manifests in environment repos.
- **Enable Automated Pruning & Self-Healing**: Guarantee Git remains the absolute single source of truth (`prune: true`, `selfHeal: true`).
- **Use Sync Waves and Hooks**: Control order of execution (e.g., Run DB migrations before deploying application pods).
- **Automate Metric-Driven Canary Analysis**: Integrate Argo Rollouts with Prometheus to trigger automatic rollbacks if error rates exceed 1%.

---

## 🔗 Related Resources
- [OpenGitOps Specification](https://opengitops.dev/)
- [Argo Project Documentation](https://argoproj.github.io/)
- [FluxCD Official Documentation](https://fluxcd.io/)

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 02 - Canary Deployments with Argo Rollouts](./02-Canary-Deployments-with-Argo-Rollouts.md) | [Index](../../../README.md) | [04 - AnalysisTemplates AnalysisRuns and Prometheus Integration →](./04-AnalysisTemplates-AnalysisRuns-and-Prometheus-Integration.md) |
