# 04 - Bicep Target Scopes: ResourceGroup, Subscription & ManagementGroup

> **Focus:** Multi-tier resource governance deployments in Azure Bicep. Deep dive into `targetScope`, deploying across scope boundaries, orchestrating Azure Landing Zones, creating Resource Groups, and assigning Azure Policies at Management Group scale.

---

## 🏛️ 1. What, Why, and History of Bicep Target Scopes

### What is a Target Scope?
The `targetScope` directive in Azure Bicep specifies the exact level within the Azure Resource Hierarchy where the root template executes. It determines which Azure Resource Providers and APIs are legally callable within that template.

### Why do We Need Multiple Target Scopes?
Not every Azure entity lives inside a Resource Group:
1. **Resource Groups do not live inside Resource Groups:** To create a Resource Group using code, your Bicep file must execute at the **Subscription** scope.
2. **Enterprise Policy & Governance:** Azure Policy definitions, Initiatives, and corporate RBAC roles are assigned at the **Management Group** scope to enforce compliance across 50 subscriptions simultaneously.
3. **Tenant Root Management:** Custom RBAC role definitions or root management groups live at the **Tenant** scope.

---

## ⚖️ 2. The 4 Bicep Target Scopes Compared

```text
┌────────────────────────────────────────────────────────────────────────────────────────┐
│                              BICEP TARGET SCOPE HIERARCHY                              │
│                                                                                        │
│   1. targetScope = 'tenant'                                                            │
│      • Commands: az deployment tenant create                                           │
│      • Scope of: Root Management Groups, Tenant-wide Custom RBAC definitions           │
│                               │                                                        │
│                               ▼                                                        │
│   2. targetScope = 'managementGroup'                                                   │
│      • Commands: az deployment mg create                                               │
│      • Scope of: Azure Policy Assignments, Policy Definitions, Enterprise RBAC         │
│                               │                                                        │
│                               ▼                                                        │
│   3. targetScope = 'subscription'                                                      │
│      • Commands: az deployment sub create                                              │
│      • Scope of: Creating Resource Groups, Microsoft Defender for Cloud plans          │
│                               │                                                        │
│                               ▼                                                        │
│   4. targetScope = 'resourceGroup' (DEFAULT)                                           │
│      • Commands: az deployment group create                                            │
│      • Scope of: VMs, Storage Accounts, VNets, Databases, AKS Clusters                │
└────────────────────────────────────────────────────────────────────────────────────────┘
```

---

## 🚀 3. Cross-Scope Orchestration: Subscription Creating Resource Groups

A common enterprise pattern is a **Subscription-level Bicep orchestrator** that creates a Resource Group and immediately invokes a child module scoped inside the newly created Resource Group:

```bicep
// main.bicep
targetScope = 'subscription' // Root executes at Subscription level!

param resourceGroupName string = 'rg-fintech-prod'
param location string = 'eastus'

// 1. Create the Resource Group at Subscription Scope
resource newResourceGroup 'Microsoft.Resources/resourceGroups@2023-07-01' = {
  name: resourceGroupName
  location: location
  tags: {
    Department: 'Fintech'
    Environment: 'Production'
  }
}

// 2. Cross-Scope Module Invocation:
// Explicitly pass 'scope: newResourceGroup' to drop into Resource Group scope!
module appInfrastructure './modules/app-resources.bicep' = {
  name: 'deploy-app-resources'
  scope: newResourceGroup // <--- Scoped inside the newly provisioned RG!
  params: {
    location: location
    storagePrefix: 'fintech'
  }
}
```

---

## 💻 4. CLI Execution Commands Across Different Scopes

Deploying at non-default scopes requires using the corresponding Azure CLI command:

```bash
# Deploying at Resource Group Scope (Default):
az deployment group create \
  --resource-group rg-apps \
  --template-file main.bicep

# Deploying at Subscription Scope (e.g., Creating Resource Groups):
az deployment sub create \
  --location eastus \
  --template-file subscription-deploy.bicep \
  --parameters resourceGroupName='rg-prod-core'

# Deploying at Management Group Scope (e.g., Enterprise Policy):
az deployment mg create \
  --management-group-id 'mg-enterprise-root' \
  --location eastus \
  --template-file policy-governance.bicep
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 03 - Bicep Modules and Code Reusability](./03-Bicep-Modules-and-Code-Reusability.md) | [Index](../../../README.md) | [05 - Decompiling ARM Templates and Bicep CLI →](./05-Decompiling-ARM-Templates-and-Bicep-CLI.md) |
