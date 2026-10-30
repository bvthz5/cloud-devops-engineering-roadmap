# 05 - Azure Arc-Enabled Data Services: Azure SQL & PostgreSQL Anywhere

> **Focus:** Running cloud PaaS database engines on-premises and across multi-cloud infrastructure. Deep dive into the Azure Arc Data Controller, Azure SQL Managed Instance on Arc, Azure Arc-enabled PostgreSQL, Directly Connected vs. Indirectly Connected modes, and evergreen automated patching.

---

## 🗄️ 1. What, Why, and History of Arc Data Services

### What are Azure Arc-Enabled Data Services?
Azure Arc-enabled Data Services allows organizations to deploy and run fully managed Azure database platform services—specifically **Azure SQL Managed Instance** and **Azure Database for PostgreSQL**—on any Kubernetes infrastructure located in on-premises datacenters, edge sites, AWS, or GCP.

### Why do Enterprises Run Arc Data Services?
1. **Cloud PaaS Capabilities on On-Premises Hardware:** DBAs get automated backups, point-in-time restore (PITR), automated zero-downtime high availability failover, and built-in metric monitoring on their own physical infrastructure.
2. **Evergreen Database Versions (Zero End-of-Life):** Eliminates expensive SQL Server version upgrades (e.g., migrating from SQL Server 2012 to 2022). Arc Data Services provides continuous, rolling updates with zero end-of-support deadlines.
3. **Data Sovereignty & Local Residency:** 100% of the customer database tables and storage disks remain inside the physical building; only billing usage and health telemetry metadata are transmitted to Azure.

---

## ⚖️ 2. Architectural Comparison: Directly Connected vs. Indirectly Connected Modes

```text
┌────────────────────────────────────────────────────────────────────────────────────────┐
│                              CONNECTIVITY MODES IN ARC DATA SERVICES                   │
├────────────────────────────────────────────────────────────────────────────────────────┤
│ MODE 1: DIRECTLY CONNECTED (Continuous Cloud Integration)                              │
│   • Continuous outbound connection to Azure ARM control plane over port 443.           │
│   • Deploy, scale, and manage databases directly from the Azure Portal GUI or CLI.     │
│   • Automated real-time log, metric, and billing synchronization.                      │
│                                                                                        │
│ MODE 2: INDIRECTLY CONNECTED (Air-Gapped & Secure Sites)                               │
│   • For defense, submarines, manufacturing plants, or air-gapped facilities.           │
│   • ZERO permanent outbound internet connection required!                             │
│   • Managed locally via local kubectl or Azure Data Studio.                            │
│   • Billing usage exported periodically to a JSON file and uploaded to Azure manually! │
└────────────────────────────────────────────────────────────────────────────────────────┘
```

---

## 🏗️ 3. The Arc Data Controller Architecture

The **Azure Arc Data Controller** is the central orchestrator running inside the local Kubernetes cluster:

```text
┌─────────────────────────────────────────────────────────────────────────────────────────┐
│                              ARC DATA CONTROLLER TOPOLOGY                               │
│                                                                                         │
│   LOCAL KUBERNETES CLUSTER (On-Premises / OpenShift / EKS)                              │
│   ┌─────────────────────────────────────────────────────────────────────────────────┐   │
│   │ DATA CONTROLLER PODS (Namespace: arc-data)                                      │   │
│   │ • API Service: Handles incoming database provisioning requests                  │   │
│   │ • In-Cluster Monitoring: Dedicated local Grafana & Kibana instances             │   │
│   │ • Bootstrapper Controller: Deploys CRDs and manages database operators          │   │
│   └────────────────────────────────────────┬────────────────────────────────────────┘   │
│                                            │                                            │
│                                            ▼ Dynamic Provisioning                       │
│   ┌─────────────────────────────────────────────────────────────────────────────────┐   │
│   │ AZURE SQL MANAGED INSTANCE POD (Namespace: arc-data)                            │   │
│   │ • Primary Pod: sql-mi-0 (Active read/write)                                     │   │
│   │ • Secondary Pod: sql-mi-1 (Always On synchronous replica for high availability) │   │
│   │ • Automated Local Backup: Continuous transactional log backups to local PVC     │   │
│   └─────────────────────────────────────────────────────────────────────────────────┘   │
└─────────────────────────────────────────────────────────────────────────────────────────┘
```

---

## 💻 4. Practical CLI: Deploying an Arc SQL Managed Instance

```bash
# 1. Deploy the Azure Arc Data Controller onto local cluster
az arcdata dc create \
  --name "dc-onprem-frankfurt" \
  --connectivity-mode "direct" \
  --resource-group "rg-hybrid-databases" \
  --location "eastus" \
  --infrastructure "onpremises" \
  --use-k8s

# 2. Deploy an Azure SQL Managed Instance on local Kubernetes
az sql mi-arc create \
  --name "sql-mi-banking-core" \
  --resource-group "rg-hybrid-databases" \
  --custom-location $(az customlocation show -g rg-hybrid-databases -n cl-onprem --query id -o tsv) \
  --cores-limit "8" \
  --cores-request "4" \
  --memory-limit "32Gi" \
  --memory-request "16Gi" \
  --license-type "BasePrice" # Leverage Azure Hybrid Benefit!
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 04 - GitOps and Configuration Management with Azure Arc](./04-GitOps-and-Configuration-Management-with-Azure-Arc.md) | [Index](../../../README.md) | [06 - Azure ExpressRoute and Azure Stack Portfolio →](./06-Azure-ExpressRoute-and-Azure-Stack-Portfolio.md) |
