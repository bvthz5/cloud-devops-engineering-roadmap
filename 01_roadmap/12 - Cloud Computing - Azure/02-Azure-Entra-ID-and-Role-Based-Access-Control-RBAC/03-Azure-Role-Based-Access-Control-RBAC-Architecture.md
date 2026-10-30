# 03 - Azure Role-Based Access Control (RBAC) Architecture

Azure Role-Based Access Control (Azure RBAC) is the authorization system built into Azure Resource Manager (ARM) that provides fine-grained access management of Azure resources. Instead of granting wide-ranging administrative privileges, RBAC allows security administrators to enforce the **Principle of Least Privilege** by granting only the exact permissions needed to perform specific job duties.

---

## 🏛️ 1. The Core RBAC Equation

Every RBAC assignment in Azure is governed by a simple three-part formula:

$$\textbf{RBAC Assignment} = \textbf{Security Principal (Who)} + \textbf{Role Definition (What)} + \textbf{Scope (Where)}$$

```text
┌────────────────────────────────────────────────────────────────────────────────────────┐
│                              THE AZURE RBAC FORMULA                                    │
│                                                                                        │
│   SECURITY PRINCIPAL (Who)      ROLE DEFINITION (What)         SCOPE (Where)           │
│   ┌───────────────────────┐     ┌───────────────────────┐     ┌───────────────────────┐│
│   │ • Human User          │     │ • Owner               │     │ • Management Group    ││
│   │ • Security Group      │  +  │ • Contributor         │  @  │ • Subscription        ││
│   │ • Service Principal   │     │ • Reader              │     │ • Resource Group      ││
│   │ • Managed Identity    │     │ • Custom Data Role    │     │ • Individual Resource ││
│   └───────────────────────┘     └───────────────────────┘     └───────────────────────┘│
│                                             │                                          │
│                                             ▼                                          │
│                                   [ ROLE ASSIGNMENT ]                                  │
│                        (Persisted in Azure Resource Manager Engine)                    │
└────────────────────────────────────────────────────────────────────────────────────────┘
```

1. **Security Principal (Who):** An object representing a human user, security group, service principal (application), or managed identity requesting access.
2. **Role Definition (What):** A collection of permissions listing allowed operations (`Actions`) and data operations (`DataActions`).
3. **Scope (Where):** The boundary level within the Azure resource hierarchy where the access applies.

---

## 🧭 2. Scope Inheritance Architecture

Permissions in Azure flow strictly **downward** through the management hierarchy. When a role is assigned at a higher scope, the permissions are automatically inherited by all child containers and resources:

```text
┌────────────────────────────────────────────────────────────────────────────────────────┐
│                                 SCOPE INHERITANCE FLOW                                 │
│                                                                                        │
│   Management Group (Role assigned here)                                                │
│         │                                                                              │
│         ▼ (Inherits downward)                                                          │
│   Subscription                                                                         │
│         │                                                                              │
│         ▼ (Inherits downward)                                                          │
│   Resource Group                                                                       │
│         │                                                                              │
│         ▼ (Inherits downward)                                                          │
│   Individual Resource (Storage Account, VM, Key Vault)                                 │
└────────────────────────────────────────────────────────────────────────────────────────┘
```

> **Inheritance Rule:** If you assign the `Reader` role to an engineer at the **Subscription** level, that engineer automatically has read access to **every Resource Group and resource** inside that subscription—even resources created weeks later!

---

## 👑 3. Core Built-In Roles Reference

Azure provides hundreds of pre-configured built-in roles. Four fundamental roles govern overall infrastructure management:

| Built-In Role | Control Plane Actions Allowed | User Access Delegation? | Typical Enterprise Use Case |
| :--- | :--- | :--- | :--- |
| **Owner** | Full read, write, create, and delete across all resources. | ✅ **Yes** (Can grant & revoke RBAC roles) | Subscription Administrator / Cloud Lead. |
| **Contributor** | Full read, write, create, and delete across all resources. | ❌ **No** (Cannot assign roles or policies) | DevOps Engineers, Infrastructure Automation. |
| **Reader** | Read-only visibility into resource configurations. | ❌ **No** (Cannot change anything) | Security Auditors, External Stakeholders. |
| **User Access Administrator** | Only manages user access, roles, and assignments. | ✅ **Yes** (Dedicated IAM administrator) | Governance & Compliance Security Admins. |

### Specialized Built-In Roles for Data & Infrastructure Teams
| Specialized Role | Target Resource Scope | Operational Capability |
| :--- | :--- | :--- |
| **Storage Blob Data Contributor** | Storage Account / Container | Read, write, and delete blobs inside Azure Data Lake / Blob Storage. |
| **Storage Blob Data Reader** | Storage Account / Container | Read-only access to datasets inside Azure Data Lake / Blob Storage. |
| **Azure Data Factory Contributor** | Resource Group / ADF | Create, manage, and execute Azure Data Factory ETL pipelines. |
| **SQL DB Contributor** | Resource Group / SQL Server | Provision and manage Azure SQL databases (does not grant data access). |
| **Key Vault Secrets User** | Key Vault | Read secret contents from Key Vault (for applications and CI/CD). |
| **Network Contributor** | Resource Group / VNet | Manage Virtual Networks, Subnets, and Network Security Groups. |

---

## ⚡ 4. Critical Architectural Concept: `Actions` vs. `DataActions`

A major source of confusion in enterprise Azure deployments is the boundary between **Control Plane** and **Data Plane**:

```text
┌────────────────────────────────────────────────────────────────────────────────────────┐
│                        CONTROL PLANE vs. DATA PLANE IN AZURE                           │
│                                                                                        │
│   CONTROL PLANE (Actions)                      DATA PLANE (DataActions)                │
│   Managed by: Azure Resource Manager (ARM)     Managed by: The Service Engine Itself   │
│   Endpoint: management.azure.com               Endpoint: *.blob.core.windows.net       │
│                                                                                        │
│   Examples:                                    Examples:                               │
│   • Create a Storage Account                   • Read a file (blob) inside a container │
│   • Resize a Virtual Machine                   • Query rows inside a SQL database      │
│   • Delete a Virtual Network                   • Read a secret from inside a Key Vault │
│   • Assign an NSG to a Subnet                  • Push an event into an Event Hub       │
└────────────────────────────────────────────────────────────────────────────────────────┘
```

> **The Classic Enterprise Trap:** 
> A user with the **Contributor** role on a storage account can change firewall settings, resize tiers, and delete the entire storage account (`Actions`), but **CANNOT read the files or blobs stored inside it**! Reading blob contents requires explicit data-plane permissions, such as **`Storage Blob Data Reader`** (`DataActions`).

---

## 💻 5. Practical Azure CLI Management

### 1. Assign Contributor Role to a User on a Resource Group
```bash
az role assignment create \
  --assignee "manoop@contoso.com" \
  --role "Contributor" \
  --scope "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/rg-data-engineering"
```

### 2. Assign Storage Data Role to a Service Principal on a Storage Account
```bash
az role assignment create \
  --assignee "11111111-2222-3333-4444-555555555555" \
  --role "Storage Blob Data Contributor" \
  --scope "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/rg-data/providers/Microsoft.Storage/storageAccounts/mydatalakeprod"
```

### 3. Assign Permissions to a Security Group (Enterprise Best Practice)
```bash
# Assign Contributor to the 'Data Engineers' group
az role assignment create \
  --assignee "99999999-8888-7777-6666-555555555555" \
  --role "Contributor" \
  --scope "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/rg-data-engineering"
```

### 4. Audit Effective Role Assignments
```bash
# List all active role assignments on a specific resource group
az role assignment list \
  --resource-group rg-data-engineering \
  --output table
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 02 - Entra ID Users Groups and Administrative Units](./02-Entra-ID-Users-Groups-and-Administrative-Units.md) | [Index](../../../README.md) | [04 - Creating Custom RBAC Roles JSON Definitions →](./04-Creating-Custom-RBAC-Roles-JSON-Definitions.md) |
