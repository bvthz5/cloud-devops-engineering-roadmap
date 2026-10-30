# Hands-On Practice - GitOps Image Automation & Secrets Management

> **Module**: GitOps Image Automation & Secrets Management

---

## 🛠 Lab: Multi-Cluster GitOps Automation with ArgoCD ApplicationSets

### Objective
In this lab, you will configure an ArgoCD ApplicationSet using a Git Generator to discover application overlays and automatically deploy them across target environment namespaces.

---

## 📋 Task 1: Create ApplicationSet Manifest (`appset.yaml`)
```yaml
apiVersion: argoproj.io/v1alpha1
kind: ApplicationSet
metadata:
  name: fleet-applications
  namespace: argocd
spec:
  generators:
    - git:
        repoURL: https://github.com/argoproj/argo-cd.git
        revision: HEAD
        directories:
          - path: applicationset/examples/git-generator-directory/apps/*
  template:
    metadata:
      name: '{{path.basename}}'
    spec:
      project: default
      source:
        repoURL: https://github.com/argoproj/argo-cd.git
        targetRevision: HEAD
        path: '{{path}}'
      destination:
        server: https://kubernetes.default.svc
        namespace: '{{path.basename}}'
      syncPolicy:
        automated:
          prune: true
          selfHeal: true
```

---

## 📋 Task 2: Apply ApplicationSet
```bash
# Apply ApplicationSet to ArgoCD cluster
kubectl apply -f appset.yaml
```

---

## 📋 Task 3: Verify Automated Application Generation
```bash
# Verify generated Applications
kubectl get applications -n argocd
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 09 - Interview QA](./09-Interview-QA.md) | [Index](../../../README.md) | [11 - MCQ →](./11-MCQ.md) |
