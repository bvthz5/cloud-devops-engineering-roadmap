# Hands-On Practice - ArgoCD Architecture, Setup & Applications

> **Module**: ArgoCD Architecture, Setup & Applications

---

## 🛠 Lab: Bootstrapping ArgoCD with Declarative Sync Policies

### Objective
In this lab, you will deploy ArgoCD into a local Minikube/K3s cluster, write a declarative Application manifest with automated pruning and self-healing, and verify drift resolution.

---

## 📋 Task 1: Install ArgoCD
```bash
# Create namespace and apply ArgoCD manifests
kubectl create namespace argocd
kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml
```

---

## 📋 Task 2: Create Declarative Application Manifest (`guestbook-app.yaml`)
```yaml
apiVersion: argoproj.io/v1alpha1
kind: Application
metadata:
  name: guestbook
  namespace: argocd
spec:
  project: default
  source:
    repoURL: 'https://github.com/argoproj/argocd-example-apps.git'
    targetRevision: HEAD
    path: guestbook
  destination:
    server: 'https://kubernetes.default.svc'
    namespace: default
  syncPolicy:
    automated:
      prune: true
      selfHeal: true
```

---

## 📋 Task 3: Apply Manifest & Verify Drift Self-Healing
```bash
# Apply application manifest
kubectl apply -f guestbook-app.yaml

# Test drift self-healing: manually delete service
kubectl delete svc guestbook-ui

# Verify ArgoCD recreates service automatically within seconds
kubectl get svc guestbook-ui
```
