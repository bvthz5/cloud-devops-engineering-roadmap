# Hands-On Practice - Release Automation, Semantic Versioning & Feature Flags

> **Module**: Release Automation, Semantic Versioning & Feature Flags

---

## 🛠 Lab: GitOps Continuous Delivery with ArgoCD

### Objective
In this lab, you will install ArgoCD into a Kubernetes cluster, deploy a sample application, configure automatic syncing and self-healing, and simulate drift resolution.

---

## 📋 Task 1: Install ArgoCD in Kubernetes
```bash
# Create namespace and install ArgoCD manifests
kubectl create namespace argocd
kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml

# Download ArgoCD CLI
curl -sSL -o argocd-linux-amd64 https://github.com/argoproj/argo-cd/releases/latest/download/argocd-linux-amd64
sudo install -m 555 argocd-linux-amd64 /usr/local/bin/argocd
```

---

## 📋 Task 2: Create GitOps Application Manifest (`app.yaml`)
```yaml
apiVersion: argoproj.io/v1alpha1
kind: Application
metadata:
  name: demo-web-app
  namespace: argocd
spec:
  project: default
  source:
    repoURL: 'https://github.com/argoproj/argocd-example-apps.git'
    targetRevision: HEAD
    path: guestbook
  destination:
    server: 'https://kubernetes.default.svc'
    namespace: demo
  syncPolicy:
    automated:
      prune: true
      selfHeal: true
```

---

## 📋 Task 3: Deploy Application & Verify Sync
```bash
# Apply application manifest
kubectl apply -f app.yaml

# Check ArgoCD application sync status
argocd app get demo-web-app
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 09 - Interview QA](./09-Interview-QA.md) | [Index](../../../README.md) | [11 - MCQ →](./11-MCQ.md) |
