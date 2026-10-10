# 02 - Azure Bicep Syntax: Parameters, Decorators, Variables & Outputs

> **Focus:** Core programming constructs of the Bicep language. Deep dive into Parameters, Decorators (`@secure()`, `@allowed()`, `@description()`), computed Variables, Resource declarations, existing resource references, Outputs, conditional deployments, and What-If pre-flight checks.

---

## 📜 1. The Core Anatomy of a Bicep File

A complete Azure Bicep file is structured into five clean logical sections:

```text
┌────────────────────────────────────────────────────────────────────────────────────────┐
│                              ANATOMY OF A BICEP FILE                                   │
├────────────────────────────────────────────────────────────────────────────────────────┤
│ 1. TARGET SCOPE: Defines deployment level (resourceGroup, subscription, managementGroup)│
│ 2. PARAMETERS: External inputs supplied by pipelines or parameter files (.bicepparam) │
│ 3. VARIABLES: Internal computed values and complex string concatenations               │
│ 4. RESOURCES & MODULES: The actual cloud infrastructure to provision                   │
│ 5. OUTPUTS: Values passed back to calling pipelines or parent modules                  │
└────────────────────────────────────────────────────────────────────────────────────────┘
```

---

## 🛡️ 2. Parameters & Parameter Decorators

Parameters accept runtime inputs. Decorators enforce strict validation and security before execution starts:

```bicep
@description('The deployment environment name.')
@allowed([
  'dev'
  'staging'
  'prod'
])
param environment string = 'dev'

@description('The primary location for all resources. Defaults to resource group location.')
param location string = resourceGroup().location

@description('Administrator password for the database. Must be marked @secure!')
@secure()
@minLength(12)
param databaseAdminPassword string

@description('Number of web server instances to scale.')
@minValue(1)
@maxValue(10)
param instanceCount int = 2
```

> **Critical Rule:** Never omit `@secure()` on secrets or passwords! The `@secure()` decorator prevents values from being logged in plain text inside Azure deployment history and CI/CD console logs.

---

## ⚙️ 3. Computed Variables & Unique String Generation

Variables store expressions and values calculated internally within the template:

```bicep
// Deterministic 13-character unique hash based on resource group ID
var uniqueSuffix = uniqueString(resourceGroup().id)

// Storage names must be globally unique, lowercase, letters and numbers only, max 24 chars
var storageAccountName = 'stor${environment}${uniqueSuffix}'

// Compute tags map applied across all resources
var standardTags = {
  Environment: environment
  ManagedBy: 'Bicep-IaC'
  CostCenter: 'Engineering-FinOps'
  DeployedDate: utcNow('yyyy-MM-dd')
}
```

---

## 🏗️ 4. Complete Bicep Infrastructure Template

Below is a production-grade template deploying an Azure Storage Account, Blob Container, and App Service Plan with conditional scaling and outputs:

```bicep
targetScope = 'resourceGroup'

// ==========================================
// PARAMETERS
// ==========================================
@allowed(['dev', 'prod'])
param environment string = 'dev'
param location string = resourceGroup().location

// ==========================================
// VARIABLES
// ==========================================
var uniqueHash = uniqueString(resourceGroup().id)
var storageName = take('stor${environment}${uniqueHash}', 24)
var appServicePlanSku = (environment == 'prod') ? 'P1v3' : 'B1'

// ==========================================
// RESOURCES
// ==========================================
// 1. Storage Account
resource storageAccount 'Microsoft.Storage/storageAccounts@2023-01-01' = {
  name: storageName
  location: location
  sku: {
    name: (environment == 'prod') ? 'Standard_ZRS' : 'Standard_LRS'
  }
  kind: 'StorageV2'
  properties: {
    minimumTlsVersion: 'TLS1_2'
    supportsHttpsTrafficOnly: true
    allowBlobPublicAccess: false
  }
  tags: {
    Environment: environment
  }
}

// 2. Child Resource: Blob Service and Container
resource blobService 'Microsoft.Storage/storageAccounts/blobServices@2023-01-01' = {
  parent: storageAccount
  name: 'default'
}

resource uploadContainer 'Microsoft.Storage/storageAccounts/blobServices/containers@2023-01-01' = {
  parent: blobService
  name: 'user-uploads'
  properties: {
    publicAccess: 'None'
  }
}

// 3. App Service Plan
resource appPlan 'Microsoft.Web/serverfarms@2023-01-01' = {
  name: 'asp-${environment}-web'
  location: location
  sku: {
    name: appServicePlanSku
  }
  kind: 'linux'
  properties: {
    reserved: true // Mandatory for Linux plans
  }
}

// ==========================================
// OUTPUTS
// ==========================================
@description('Primary Blob Service REST Endpoint')
output blobEndpoint string = storageAccount.properties.primaryEndpoints.blob

@description('Resource ID of the provisioned App Service Plan')
output appPlanId string = appPlan.id
```

---

## 🔍 5. Pre-Flight What-If Validation in Azure CLI

Never deploy blind! The **What-If** command calculates the differential impact of your Bicep file against existing cloud resources before making any live changes:

```bash
# Execute pre-flight What-If check
az deployment group what-if \
  --resource-group rg-production-app \
  --template-file main.bicep \
  --parameters environment=prod

# Legend of What-If outputs:
#   + Create (Green): New resource will be provisioned
#   ~ Modify (Purple): Existing resource property will be altered
#   - Delete (Orange): Resource will be destroyed (if in Complete mode)
#   = NoChange (Grey): Resource configuration already matches Bicep declaration
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 01 - Infrastructure as Code on Azure ARM vs Bicep](./01-Infrastructure-as-Code-on-Azure-ARM-vs-Bicep.md) | [Index](../../../README.md) | [03 - Bicep Modules and Code Reusability →](./03-Bicep-Modules-and-Code-Reusability.md) |
