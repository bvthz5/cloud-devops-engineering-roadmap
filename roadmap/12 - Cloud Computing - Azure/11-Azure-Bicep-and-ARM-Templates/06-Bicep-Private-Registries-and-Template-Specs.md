# 06 - Bicep Private Registries (ACR) & Template Specs Architecture

> **Focus:** Enterprise module distribution and governance in Azure Bicep. Deep dive into Private Module Registries hosted in Azure Container Registry (ACR), OCI artifact storage, semantic versioning, `bicepconfig.json` module aliases, and Azure Template Specs.

---

## 🏛️ 1. What, Why, and History of Enterprise Bicep Sharing

### What are Private Registries and Template Specs?
- **Private Bicep Registries:** Uses Azure Container Registry (ACR) to store and distribute version-controlled Bicep modules as standard Open Container Initiative (OCI) artifacts across the entire organization.
- **Template Specs:** Native Azure Resource Manager resources that package an ARM/Bicep template inside an Azure Resource Group, making it consumable via the Azure Portal, REST API, or other templates.

### Why do Organizations Need a Centralized Private Registry?
1. **Central Platform Engineering Governance:** The central cloud platform team builds, tests, and certifies corporate-approved modules (e.g., hardened AKS clusters with mandated security policies). Developers cannot modify the module code; they simply consume published versions.
2. **Immutable Semantic Versioning:** Modules are published with strict version tags (`v1.0.0`, `v1.1.0`, `v2.0.0`). Breaking changes in `v2.0.0` will never break existing development teams consuming `v1.0.0`.
3. **Cross-Repository Code Sharing:** Avoids fragile Git submodules or copy-pasting `.bicep` files across dozens of Git repositories.

---

## ⚖️ 2. Architectural Comparison: Bicep Private Registry vs. Template Specs

| Feature / Criteria | Bicep Private Registry (ACR) | Azure Template Specs |
| :--- | :--- | :--- |
| **Underlying Technology** | **Azure Container Registry (OCI Artifact)** | Native ARM Resource (`Microsoft.Resources/templateSpecs`)|
| **Primary Audience** | **DevOps & Platform Engineers (IaC code)** | IT Administrators & Self-Service Portal users |
| **Deployment Mechanism** | Referenced directly in Bicep (`br:...`) | Deployed via Azure Portal, CLI, or ARM API |
| **Versioning Model** | Semantic OCI image tags (`v1.2.0`) | Template Spec Version sub-resource (`1.0.0`) |
| **Tooling Experience** | Full VS Code IntelliSense & autocomplete | Limited code authoring integration |
| **Recommendation** | **Best Practice for CI/CD & Bicep code** | Best for Portal-based self-service deployments |

---

## 🏗️ 3. Private Module Registry Architecture & OCI Flow

```text
┌────────────────────────────────────────────────────────────────────────────────────────┐
│                              BICEP PRIVATE REGISTRY ARCHITECTURE                       │
│                                                                                        │
│   CENTRAL PLATFORM TEAM REPOSITORY                                                     │
│   ├── modules/networking/vnet.bicep                                                    │
│   └── Publishes module to corporate ACR via Azure CLI:                                 │
│       az bicep publish --file vnet.bicep --target br:acrcorp.azurecr.io/bicep/vnet:1.0.0│
│                               │                                                        │
│                               ▼                                                        │
│   AZURE CONTAINER REGISTRY (acrcorp.azurecr.io)                                        │
│   ├── Repository: bicep/vnet                                                           │
│   │   ├── Tag: 1.0.0 (OCI Artifact manifest + compiled ARM JSON payload)               │
│   │   └── Tag: 2.0.0                                                                   │
│   └── RBAC: AcrPull permission granted to application development service principals    │
│                               │                                                        │
│                               ▼ Pulled during CI/CD compilation                        │
│   APPLICATION TEAM PIPELINE (main.bicep)                                               │
│   module vnet 'br:acrcorp.azurecr.io/bicep/vnet:1.0.0' = {                             │
│     name: 'deploy-vnet'                                                                │
│     params: { environment: 'prod' }                                                    │
│   }                                                                                    │
└────────────────────────────────────────────────────────────────────────────────────────┘
```

---

## ⚙️ 4. Simplifying Module Paths with `bicepconfig.json` Aliases

Instead of writing long ACR server URLs across all templates, configure a clean alias in `bicepconfig.json`:

```json
{
  "moduleAliases": {
    "br": {
      "enterprise": {
        "registry": "acrcorp.azurecr.io",
        "modulePath": "bicep/modules"
      }
    }
  }
}
```

### Clean Consumption in Bicep:
```bicep
// Now you can invoke modules using your clean enterprise alias!
module coreNetwork 'br/enterprise:vnet:1.0.0' = {
  name: 'deploy-core-network'
  params: {
    vnetName: 'vnet-prod-core'
    location: 'eastus'
  }
}
```

---

## 💻 5. Practical CLI: Publishing and Consuming Bicep Modules

```bash
# 1. Publish a module to Azure Container Registry
az bicep publish \
  --file modules/storage.bicep \
  --target "br:acrcorp.azurecr.io/bicep/modules/storage:1.0.0"

# 2. Verify the published OCI module artifact in ACR
az acr repository list --name acrcorp --output table
az acr repository show-tags --name acrcorp --repository bicep/modules/storage --output table
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 05 - Decompiling ARM Templates and Bicep CLI](./05-Decompiling-ARM-Templates-and-Bicep-CLI.md) | [Index](../../../README.md) | [07 - Real World Scenarios →](./07-Real-World-Scenarios.md) |
