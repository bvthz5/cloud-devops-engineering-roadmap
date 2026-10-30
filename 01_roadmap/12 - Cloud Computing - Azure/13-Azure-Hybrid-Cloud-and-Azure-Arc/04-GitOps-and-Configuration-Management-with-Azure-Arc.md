# 04 - GitOps with Azure Arc: Declarative Flux v2 Continuous Delivery

> **Focus:** At-scale Kubernetes application deployment and drift correction using GitOps. Deep dive into the Azure Arc Flux v2 extension, pull-based vs. push-based CD, Kustomize overlays, Helm release controllers, and automated configuration drift reconciliation.

---

## 🐙 1. What, Why, and History of GitOps with Azure Arc

### What is GitOps?
GitOps is an operational framework that uses a Git repository as the **single source of truth** for declaratively defined infrastructure and application states. An in-cluster software agent continuously compares the live cluster state against the declared Git state, automatically synchronizing changes and reversing unauthorized manual modifications.

### Why does Azure Arc Use Pull-Based GitOps Instead of Push CI/CD?
1. **Zero Cluster Credential Exposure:** Traditional push CI/CD pipelines require storing high-privilege `kubeconfig` administrator credentials inside external CI runners (like GitHub Actions or Azure DevOps). In GitOps, the in-cluster agent pulls configurations outbound—**no cluster credentials ever leave the network!**
2. **Automated Drift Correction:** If an operator manually edits a pod replica count or deletes a secret via `kubectl`, the GitOps operator detects the discrepancy within seconds and automatically re-applies the Git declaration, obliterating configuration drift.
3. **Multi-Cluster Fleet Orchestration:** Pushing code to 200 edge retail store clusters one by one is brittle. With Arc GitOps, linking all 200 clusters to a single Git repository deploys updates across the entire global fleet simultaneously.

### 📜 History & Evolution:
- **2019:** Azure Arc launches with early Flux v1 (Weaveworks) configuration management.
- **2022–Present:** Azure Arc adopts **Flux v2**, an open-source CNCF Graduated project built on modular Kubernetes controllers (`source-controller`, `kustomize-controller`, `helm-controller`).

---

## 🏗️ 2. The Arc Flux v2 GitOps Reconciliation Loop

```text
┌────────────────────────────────────────────────────────────────────────────────────────┐
│                              ARCOPS FLUX V2 RECONCILIATION FLOW                        │
│                                                                                        │
│   CENTRAL GIT REPOSITORY (GitHub / Azure Repos)                                        │
│   https://github.com/contoso-bank/fleet-infra-configs                                  │
│   ├── clusters/production/                                                             │
│   │   ├── kustomization.yaml                                                           │
│   │   └── ingress-nginx.yaml                                                           │
│                               │                                                        │
│                               ▼ Pulls over HTTPS / SSH (Read-Only)                     │
│   KUBERNETES CLUSTER (AWS EKS / On-Prem OpenShift / AKS)                               │
│   ┌────────────────────────────────────────────────────────────────────────────────┐   │
│   │ FLUX V2 CONTROLLERS (flux-system namespace)                                    │   │
│   │                                                                                │   │
│   │ 1. source-controller:                                                          │   │
│   │    • Polls Git repo every 1m; pulls latest commit hash and builds artifact tgz.│   │
│   │                                                                                │   │
│   │ 2. kustomize-controller:                                                       │   │
│   │    • Renders Kustomize overlays; applies manifests directly to kube-apiserver. │   │
│   │                                                                                │   │
│   │ 3. DRIFT DETECTION & HEALING:                                                  │   │
│   │    • Compares live etcd state against Git commit declaration.                  │   │
│   │    • If a developer manually altered replicas: OVERWRITES TO MATCH GIT!        │   │
│   └────────────────────────────────────────────────────────────────────────────────┘   │
└────────────────────────────────────────────────────────────────────────────────────────┘
```

---

## 💻 3. Practical CLI: Deploying a Flux v2 GitOps Configuration via Azure Arc

```bash
# 1. Install the k8s-configuration extension
az extension add --name k8s-configuration

# 2. Deploy a Flux v2 GitOps configuration to an Arc-enabled Kubernetes cluster
az k8s-configuration flux create \
  --name "flux-system-core" \
  --cluster-name "eks-production-cluster-aws" \
  --resource-group "rg-multicloud-k8s" \
  --cluster-type "connectedClusters" \
  --scope cluster \
  --namespace flux-system \
  --url "https://github.com/contoso-bank/fleet-infra-configs" \
  --branch "main" \
  --kustomization name="cluster-base" path="./clusters/production" prune=true sync-interval=3m

# 3. Verify GitOps synchronization status
az k8s-configuration flux show \
  --name "flux-system-core" \
  --cluster-name "eks-production-cluster-aws" \
  --resource-group "rg-multicloud-k8s" \
  --cluster-type "connectedClusters" \
  --query complianceState -o tsv
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 03 - Azure Arc Enabled Kubernetes Clusters](./03-Azure-Arc-Enabled-Kubernetes-Clusters.md) | [Index](../../../README.md) | [05 - Azure Arc Enabled Data Services →](./05-Azure-Arc-Enabled-Data-Services.md) |
