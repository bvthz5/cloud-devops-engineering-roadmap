# 03 - Bicep Modules: Composable Infrastructure & Code Reusability

> **Focus:** Modular software engineering patterns applied to Infrastructure as Code in Azure Bicep. Deep dive into creating standalone modules, parameter passing, consuming module outputs, implicit vs. explicit dependencies (`dependsOn`), and constructing maintainable enterprise architectures.

---

## 🧩 1. What, Why, and History of Bicep Modules

### What is a Bicep Module?
A Bicep Module is simply a standard Bicep file that is referenced and invoked from another Bicep file using the `module` keyword. Modules encapsulate complex resource configurations into reusable, self-contained components.

### Why do Enterprise Teams Mandate Modular IaC?
1. **Eliminate Code Duplication (DRY Principle):** Rather than re-declaring a 100-line Virtual Network with 5 subnets across 20 application repositories, teams build one standardized `vnet.bicep` module and invoke it everywhere.
2. **Separation of Concerns:** Networking teams own `modules/networking.bicep`, database DBAs own `modules/database.bicep`, and application developers simply instantiate them inside `main.bicep`.
3. **Automated Dependency Graphing:** The Bicep compiler automatically analyzes module input/output relationships and constructs an optimal parallel execution graph. Independent modules deploy concurrently; dependent modules wait automatically.

---

## 🏗️ 2. Modular Architecture & Directory Structure

```text
infrastructure/
├── main.bicep                     <-- Orchestration root file
├── main.bicepparam                <-- Environment parameter values
└── modules/                       <-- Reusable component library
    ├── networking.bicep           <-- Provisions VNet, Subnets & NSGs
    ├── keyvault.bicep             <-- Provisions Key Vault & Access Policies
    └── compute.bicep              <-- Provisions VM Scale Set or AKS
```

---

## 📜 3. Building Reusable Modules: Code Walkthrough

### Step 1: The Child Module (`modules/networking.bicep`):
```bicep
// modules/networking.bicep
param vnetName string
param location string
param addressPrefix string = '10.0.0.0/16'

resource vnet 'Microsoft.Network/virtualNetworks@2023-05-01' = {
  name: vnetName
  location: location
  properties: {
    addressSpace: {
      addressPrefixes: [addressPrefix]
    }
    subnets: [
      {
        name: 'snet-app'
        properties: { addressPrefix: '10.0.1.0/24' }
      }
      {
        name: 'snet-db'
        properties: { addressPrefix: '10.0.2.0/24' }
      }
    ]
  }
}

// Export subnet IDs for parent consumption
output vnetId string = vnet.id
output appSubnetId string = vnet.properties.subnets[0].id
output dbSubnetId string = vnet.properties.subnets[1].id
```

### Step 2: The Parent Orchestrator (`main.bicep`):
```bicep
// main.bicep
targetScope = 'resourceGroup'

param environment string = 'prod'
param location string = resourceGroup().location

// 1. Invoke the Networking Module
module network './modules/networking.bicep' = {
  name: 'deploy-networking-${environment}'
  params: {
    vnetName: 'vnet-${environment}-core'
    location: location
    addressPrefix: '10.10.0.0/16'
  }
}

// 2. Invoke the Database Module (Implicit Dependency!)
// Because we pass 'network.outputs.dbSubnetId', Bicep automatically waits
// for the networking module to finish provisioning BEFORE starting the database!
module database './modules/database.bicep' = {
  name: 'deploy-database-${environment}'
  params: {
    serverName: 'sql-${environment}-${uniqueString(resourceGroup().id)}'
    location: location
    subnetId: network.outputs.dbSubnetId // <--- Implicit Dependency Link!
  }
}
```

---

## 🔄 4. Implicit vs. Explicit Dependencies

| Dependency Type | Syntax | Behavior | Best Practice |
| :--- | :--- | :--- | :--- |
| **Implicit Dependency** | `subnetId: network.outputs.id` | Bicep inspects property references and automatically orders deployments. | **Preferred Standard** (Cleanest and self-documenting) |
| **Explicit Dependency** | `dependsOn: [ network ]` | Manually forces Module B to wait for Module A when no direct property links exist. | Use only for sequencing non-linked resources |

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 02 - Bicep Language Syntax Parameters Variables and Outputs](./02-Bicep-Language-Syntax-Parameters-Variables-and-Outputs.md) | [Index](../../../README.md) | [04 - Target Scopes ResourceGroup Subscription ManagementGroup →](./04-Target-Scopes-ResourceGroup-Subscription-ManagementGroup.md) |
