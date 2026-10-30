# 02 - Azure Key Vault: Vault Access Policies vs. Azure RBAC Permissions

> **Focus:** Authorization models for Azure Key Vault data plane operations. Deep dive into the legacy Vault Access Policies model vs. the modern Azure Role-Based Access Control (RBAC) model, built-in Key Vault data roles, item-level secret scoping, and Privileged Identity Management (PIM) integration.

---

## ⚖️ 1. What, Why, and History of Key Vault Authorization Models

### The Two Authorization Models
When deploying an Azure Key Vault, you must select one of two mutually exclusive permission models for data plane operations (reading, writing, deleting secrets, keys, and certs):
1. **Vault Access Policies (Legacy):** Access rules defined directly inside the Key Vault resource properties mapping an Entra ID identity to a bundle of permissions.
2. **Azure RBAC (Modern Standard):** Utilizes native Azure Resource Manager RBAC role assignments, evaluated at the Management Group, Subscription, Resource Group, Vault, or individual item scope.

### Why did Microsoft Transition to Azure RBAC?
1. **Item-Level Granular Scoping:** Vault Access Policies apply to the **entire Key Vault**. If you grant an application "Get Secret", it can read *every single secret in that vault*. With Azure RBAC, you can assign `Key Vault Secrets User` scoped strictly to **one single secret** (e.g., `/secrets/DatabasePassword`), keeping other secrets invisible.
2. **Inheritance & Centralized Management:** Azure RBAC allows assigning security teams `Key Vault Reader` at the Subscription or Management Group level, propagating down to all vaults automatically.
3. **Privileged Identity Management (PIM):** Enables Just-In-Time (JIT) access requests and time-limited role activations for accessing production keys, fully audited.

---

## 📊 2. Architectural Comparison: Access Policies vs. Azure RBAC

```text
┌────────────────────────────────────────────────────────────────────────────────────────┐
│                              KEY VAULT AUTHORIZATION COMPARISON                        │
├────────────────────────────────────────────────────────────────────────────────────────┤
│ MODEL A: VAULT ACCESS POLICIES (LEGACY)                                                │
│   • Managed inside Key Vault properties (hard limit: max 1,024 policy entries).        │
│   • All-or-Nothing Scope: Applies vault-wide; cannot restrict to single secret.        │
│   • Decoupled from Azure ARM RBAC; does not inherit from Subscription/Resource Group.  │
│                                                                                        │
│ MODEL B: AZURE RBAC AUTHORIZATION (RECOMMENDED)                                        │
│   • Managed via standard Azure RBAC control plane (IAM blade).                         │
│   • Item-Level Scoping: Can assign permissions to an individual secret or key!         │
│   • Supports Management Group inheritance, Privileged Identity Management (PIM),       │
│     and Azure Policy enforcement.                                                      │
└────────────────────────────────────────────────────────────────────────────────────────┘
```

| Criteria | Vault Access Policies (Legacy) | Azure RBAC Model (Modern Standard) |
| :--- | :--- | :--- |
| **Minimum Granularity Scope**| Entire Key Vault only | **Individual Secret, Key, or Certificate** |
| **Hierarchy Inheritance** | ❌ None (Defined per-vault) | ✅ **Yes (MG ➔ Sub ➔ RG ➔ Vault ➔ Item)** |
| **PIM (Just-In-Time) Support**| ❌ No | ✅ **Yes (Full PIM workflow support)** |
| **Maximum Assignments Limit**| 1,024 entries per vault | Standard Azure RBAC assignment limits |
| **Separation of Control/Data**| Blended | Strict separation of Control vs. Data plane |
| **Enterprise Recommendation** | Deprecated for new designs | **Mandatory Microsoft Best Practice** |

---

## 🛡️ 3. Built-In Key Vault RBAC Roles Reference

Under the Azure RBAC model, Microsoft provides dedicated fine-grained roles:

| Built-In Role Name | Data Plane Scope & Permissions | Typical Assignee |
| :--- | :--- | :--- |
| **`Key Vault Administrator`** | Full control over control plane AND all secrets, keys, and certs | Cloud Security Engineers |
| **`Key Vault Secrets Officer`** | Create, update, read, and delete secrets | DevOps CI/CD Pipelines |
| **`Key Vault Secrets User`** | **Read / Get secret contents only** | Application Workloads & Managed Identities |
| **`Key Vault Crypto Officer`** | Manage cryptographic keys (create, rotate, import) | Cryptographic Architects |
| **`Key Vault Crypto User`** | Perform cryptographic operations (encrypt, decrypt, wrap) | Database Transparent Data Encryption (TDE) |
| **`Key Vault Certificates Officer`**| Manage full lifecycle of TLS/SSL certificates | Web Platform Engineers |

---

## 💻 4. Practical CLI: Scoping Access to a Single Secret via Azure RBAC

```bash
# 1. Enable Azure RBAC Authorization on the Key Vault
az keyvault update \
  --name "kv-corp-prod-eastus" \
  --resource-group "rg-security-prod" \
  --enable-rbac-authorization true

# 2. Grant an Application Managed Identity access to ONE specific secret only!
APP_SP_ID=$(az ad sp list --display-name "api-payment-service" --query "[0].id" -o tsv)
SECRET_ID=$(az keyvault secret show --vault-name "kv-corp-prod-eastus" --name "PaymentGatewayApiKey" --query id -o tsv)

az role assignment create \
  --assignee-object-id $APP_SP_ID \
  --assignee-principal-type "ServicePrincipal" \
  --role "Key Vault Secrets User" \
  --scope $SECRET_ID

# The application can now read 'PaymentGatewayApiKey', but CANNOT read any other secret in the vault!
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 01 - Azure Key Vault Architecture Secrets Keys Certificates](./01-Azure-Key-Vault-Architecture-Secrets-Keys-Certificates.md) | [Index](../../../README.md) | [03 - Azure Policy Architecture Definitions Initiatives Assignments →](./03-Azure-Policy-Architecture-Definitions-Initiatives-Assignments.md) |
