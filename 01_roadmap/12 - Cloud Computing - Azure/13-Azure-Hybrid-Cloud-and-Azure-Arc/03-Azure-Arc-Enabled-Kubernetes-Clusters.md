# 03 - Azure Arc-Enabled Kubernetes: Multi-Cloud CNCF Cluster Governance

> **Focus:** Multi-cloud Kubernetes management via Azure Arc. Deep dive into attaching CNCF-certified Kubernetes clusters (AWS EKS, Google Cloud GKE, Red Hat OpenShift, Tanzu, bare-metal k3s), the internal `azure-arc` namespace agent pods, Azure Policy for Kubernetes (OPA Gatekeeper), and Cluster Connect proxying.

---

## ☸️ 1. What, Why, and History of Arc-Enabled Kubernetes

### What is Azure Arc-Enabled Kubernetes?
Azure Arc-enabled Kubernetes enables platform engineering teams to attach and manage any CNCF-certified Kubernetes cluster—regardless of whether it runs on AWS EKS, Google Cloud GKE, on-premises VMware Tanzu, Red Hat OpenShift, or edge bare metal—directly through the Azure Resource Manager (ARM) control plane.

### Why do Cloud Platform Teams Connect External Clusters?
1. **Taming Multi-Cloud Kubernetes Sprawl:** Organizations end up with 30 clusters across AWS, Azure, and on-premises datacenters. Arc provides a single pane of glass to view node health, Kubernetes versions, and pod capacity across all clouds.
2. **Standardized Policy Enforcement (OPA Gatekeeper):** Enforce strict admission control rules (e.g., "Block containers running as root", "Enforce CPU/memory limits", "Require signed images from corporate ACR") uniformly across EKS, GKE, and AKS.
3. **Cluster Connect (Secure Remote Management):** Allows engineers to run `az connectedk8s proxy` and execute `kubectl` commands against on-premises firewalled clusters from anywhere in the world **without opening inbound firewall ports or configuring public VPNs!**

---

## 🏗️ 2. The Internal Arc Kubernetes Agent Pods (`azure-arc` Namespace)

When a cluster connects to Azure Arc, Helm charts deploy a lightweight suite of operator pods inside the dedicated `azure-arc` namespace:

```text
┌────────────────────────────────────────────────────────────────────────────────────────┐
│                              AZURE-ARC NAMESPACE OPERATOR PODS                         │
├────────────────────────────────────────────────────────────────────────────────────────┤
│ • clusteridentityoperator: Maintains secure TLS mutual authentication (mTLS) with Azure│
│ • config-agent: Pulls GitOps Flux v2 configurations from Azure and applies them locally│
│ • controller-manager: Custom Resource Definition (CRD) controller managing Arc states  │
│ • clusterconnect-agent: Reverse proxy enabling inbound 'kubectl' via outbound HTTPS 443│
│ • metrics-agent: Collects cluster health and node CPU/memory telemetry for Azure Monitor│
└────────────────────────────────────────────────────────────────────────────────────────┘
```

---

## 🌐 3. Cluster Connect: Zero-Inbound-Port Remote Access

```text
┌─────────────────────────────────────────────────────────────────────────────────────────┐
│                              CLUSTER CONNECT REVERSE PROXY FLOW                         │
│                                                                                         │
│   DEVELOPER LAPTOP                                           AZURE RELAY SERVICE (CLOUD)│
│   ┌───────────────────────────┐                              ┌────────────────────────┐ │
│   │ Developer runs:           │                              │ Azure Relay WCF/Hybrid │ │
│   │ kubectl get pods          ├─────────────────────────────>│ Endpoint (Public Edge) │ │
│   └───────────────────────────┘                              └───────────▲────────────┘ │
│                                                                          │              │
│                                                       Outbound HTTPS 443 │ Long-Poll    │
│   ON-PREMISES FIREWALLED CLUSTER (Behind Corporate NAT)                  │ Connection   │
│   ┌──────────────────────────────────────────────────────────────────────┴────────────┐ │
│   │ Pod: clusterconnect-agent (azure-arc namespace)                                   │ │
│   │ • Holds open an outbound TLS websocket connection to Azure Relay.                 │ │
│   │ • Receives kubectl API call payload from Azure Relay; passes to kube-apiserver.   │ │
│   │ • Returns pod status response back up the open websocket stream!                  │ │
│   └───────────────────────────────────────────────────────────────────────────────────┘ │
└─────────────────────────────────────────────────────────────────────────────────────────┘
```

---

## 💻 4. Practical CLI: Connecting an AWS EKS Cluster to Azure Arc

```bash
# 1. Install required Azure CLI extensions
az extension add --name connectedk8s
az extension add --name k8s-extension

# 2. Connect the active Kubernetes cluster context to Azure Arc
az connectedk8s connect \
  --name "eks-production-cluster-aws" \
  --resource-group "rg-multicloud-k8s" \
  --location "eastus" \
  --tags "CloudProvider=AWS,Environment=Production,Platform=EKS"

# 3. Verify connection status in Azure
az connectedk8s show \
  --name "eks-production-cluster-aws" \
  --resource-group "rg-multicloud-k8s" \
  --query connectivityStatus -o tsv
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 02 - Azure Arc Enabled Servers Architecture and Onboarding](./02-Azure-Arc-Enabled-Servers-Architecture-and-Onboarding.md) | [Index](../../../README.md) | [04 - GitOps and Configuration Management with Azure Arc →](./04-GitOps-and-Configuration-Management-with-Azure-Arc.md) |
