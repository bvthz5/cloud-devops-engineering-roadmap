# Troubleshooting Guide - GitOps Principles & Workflow

> **Module**: GitOps Principles & Workflow

---

## 🔍 Common Issue 1: ArgoCD App Stuck in `OutOfSync` / Infinite Sync Loop

### Symptom
ArgoCD application displays `OutOfSync` status indefinitely even after manual sync attempts, causing continuous resource recreation.

### Root Cause & Resolution
1. **Mutating Webhooks / Controllers**: A Kubernetes operator (e.g., Cert-Manager or HPA) dynamically mutates resource fields (`spec.replicas` or `metadata.annotations`) that differ from the Git manifest.
2. **Ignore Differences Policy**: Add an `ignoreDifferences` block to the ArgoCD Application spec:
   ```yaml
   spec:
     ignoreDifferences:
       - group: apps
         kind: Deployment
         jsonPointers:
           - /spec/replicas
   ```

---

## 🔍 Common Issue 2: Flux Kustomization Reconciliation Failure (`DependencyNotReady`)

### Symptom
Flux Kustomization resource reports `Kustomization reconciliation failed: dependency 'flux-system/infrastructure' is not ready`.

### Root Cause & Resolution
1. **Unmet Dependency Order**: The application Kustomization relies on CRDs or controllers (e.g., NGINX Ingress) that have not finished deploying.
2. **Health Check Wait Configuration**: Ensure the parent infrastructure Kustomization has `wait: true` and appropriate timeout values so child Kustomizations block until CRDs are established.
