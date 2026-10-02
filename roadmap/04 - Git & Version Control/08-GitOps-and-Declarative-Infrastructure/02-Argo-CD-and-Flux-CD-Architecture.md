# 02 - Argo CD and Flux CD Architecture

## 1. Argo CD Core Architecture

Argo CD implements GitOps via a declarative Kubernetes Custom Resource Definition (**CRD**): `Application`.

```yaml
apiVersion: argoproj.io/v1alpha1
kind: Application
metadata:
  name: payment-service
  namespace: argocd
spec:
  project: default
  source:
    repoURL: 'https://github.com/company/k8s-manifests.git'
    targetRevision: HEAD
    path: overlays/production
  destination:
    server: 'https://kubernetes.default.svc'
    namespace: payments
  syncPolicy:
    automated:
      prune: true      # Delete resources removed from Git
      selfHeal: true   # Overwrite manual out-of-band kubectl changes
```

---

## 2. Flux CD: Lightweight Controller Toolkit
Flux CD breaks GitOps into decoupled micro-controllers:
- `source-controller`: Clones Git repos, Helm charts, and OCI registries.
- `kustomize-controller`: Applies Kustomize overlays.
- `helm-controller`: Manages Helm releases declaratively.

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [01 - GitOps Principles](./01-GitOps-Core-Principles-and-Pull-vs-Push.md) | [README](./README.md) | [03 - Repo Topologies](./03-Repository-Topologies-Monorepo-vs-Polyrepo.md) |
