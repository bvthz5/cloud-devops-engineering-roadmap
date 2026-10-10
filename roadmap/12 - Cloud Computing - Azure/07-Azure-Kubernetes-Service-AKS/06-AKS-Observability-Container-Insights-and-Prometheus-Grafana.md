# 06 - AKS Observability: Container Insights, Managed Prometheus & Managed Grafana

> **Focus:** Full-stack observability for Azure Kubernetes Service. Deep dive into Container Insights (Azure Monitor Agent), native scraping with Azure Managed Prometheus, visualization via Azure Managed Grafana, control plane audit logging, and resolving pod OOMKilled/CrashLoopBackOff alerts.

---

## 📊 1. What, Why, and History of AKS Observability

### What is AKS Observability?
AKS Observability provides real-time health telemetry, performance metrics, container stdout/stderr logs, and distributed network tracing across both the managed Kubernetes control plane and user worker node pools.

### Why do Cloud-Native Clusters Require Dedicated Monitoring?
1. **Dynamic Ephemerality:** Pods are continuously created, scaled, and destroyed across multiple nodes. Traditional static VM monitoring agents fail to track ephemeral container lifecycles.
2. **Multi-Tenant Visibility:** Identifying which namespace, pod, or container is exhausting node memory or saturating CPU quotas.
3. **Control Plane Auditing:** Regulatory standards require tracking who executed `kubectl exec`, who changed role bindings, and when secrets were accessed.

### 📜 History & Evolution:
- **2018:** Microsoft introduces **Container Insights** based on the legacy Operations Management Suite (OMS) Log Analytics agent. Every node ran an `omsagent` daemonset container with high RAM overhead.
- **2021:** Transition to the lighter **Azure Monitor Agent (AMA)** container addon, reducing node resource consumption by 60%.
- **2022–2023:** Microsoft introduces **Azure Managed Prometheus** and **Azure Managed Grafana**. Clusters can now natively scrape Prometheus metrics endpoints without hosting self-managed Prometheus servers or persistent disk storage.

---

## 🏗️ 2. Architectural Topology of the AKS Observability Stack

```text
┌─────────────────────────────────────────────────────────────────────────────────────────┐
│                                AKS OBSERVABILITY ARCHITECTURE                           │
│                                                                                         │
│   AKS CLUSTER INFRASTRUCTURE                               MONITORING BACKEND (AZURE)   │
│   ┌───────────────────────────────────────────────┐        ┌─────────────────────────┐  │
│   │ MANAGED CONTROL PLANE                         │        │ LOG ANALYTICS WORKSPACE │  │
│   │ • kube-apiserver audit logs                   ├───────>│ • ContainerLogV2        │  │
│   │ • kube-controller-manager logs                │        │ • KubeEvents            │  │
│   │ • cluster-autoscaler logs                     │        │ • KubePodInventory      │  │
│   └───────────────────────────────────────────────┘        └─────────────────────────┘  │
│                                                                        │                │
│   ┌───────────────────────────────────────────────┐                    ▼                │
│   │ WORKER NODE POOLS                             │        ┌─────────────────────────┐  │
│   │ • Azure Monitor Agent (AMA DaemonSet)         │        │ AZURE MANAGED PROMETHEUS│  │
│   │   Scrapes container stdout/stderr & node sys  │        │ • PromQL Query Engine   │  │
│   │ • Managed Prometheus Collector DaemonSet      ├───────>│ • 18-month metric store │  │
│   │   Scrapes /metrics endpoints on port 9090     │        └───────────┬─────────────┘  │
│   └───────────────────────────────────────────────┘                    │                │
│                                                                        ▼                │
│                                                            ┌─────────────────────────┐  │
│                                                            │  AZURE MANAGED GRAFANA  │  │
│                                                            │  • Pre-built AKS Boards │  │
│                                                            │  • Pod Resource Quotas  │  │
│                                                            │  • Cluster Autoscaling  │  │
└────────────────────────────────────────────────────────────┴─────────────────────────┘──┘
```

---

## 🔍 3. Essential KQL Queries for AKS Troubleshooting in Log Analytics

### Query 1: Find All Pods Terminated by OOMKilled (Out of Memory):
```kusto
KubePodInventory
| where PodStatus == "Failed" or ContainerStatus == "Terminated"
| extend ExitReason = tostring(parse_json(ContainerStatusReason).reason)
| where ExitReason == "OOMKilled"
| project TimeGenerated, Namespace, Name, ContainerName, ExitReason, ControllerName
| order by TimeGenerated desc
```

### Query 2: Retrieve Recent Error Logs for a Failing Microservice:
```kusto
ContainerLogV2
| where PodNamespace == "production"
| where PodName startswith "payment-service"
| where LogMessage has_any ("error", "exception", "fatal", "panic")
| project TimeGenerated, PodName, ContainerName, LogMessage
| order by TimeGenerated desc
| take 100
```

---

## 💻 4. CLI Setup: Enabling Managed Prometheus & Grafana on AKS

```bash
# 1. Create Azure Monitor Workspace for Prometheus metrics
az monitor account create \
  --name amw-aks-prod \
  --resource-group rg-monitoring \
  --location eastus

# 2. Create Azure Managed Grafana instance
az grafana create \
  --name grafana-aks-prod \
  --resource-group rg-monitoring \
  --location eastus

# 3. Enable Managed Prometheus on AKS and link with Grafana
az aks update \
  --name aks-cluster-prod \
  --resource-group rg-aks \
  --enable-azure-monitor-metrics \
  --azure-monitor-workspace-resource-id $(az monitor account show -n amw-aks-prod -g rg-monitoring --query id -o tsv) \
  --grafana-resource-id $(az grafana show -n grafana-aks-prod -g rg-monitoring --query id -o tsv)
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 05 - AKS Storage Azure Disks Azure Files and CSI Drivers](./05-AKS-Storage-Azure-Disks-Azure-Files-and-CSI-Drivers.md) | [Index](../../../README.md) | [07 - Real World Scenarios →](./07-Real-World-Scenarios.md) |
