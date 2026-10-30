# 04 - Creating Custom RBAC Roles: JSON Schema & Actions vs. DataActions

> **Focus:** Fine-grained authorization, Custom Role JSON definition schema, Control Plane (`Actions`) vs. Data Plane (`DataActions`), and AssignableScopes.

---

## 📜 1. The Anatomy of a Custom Role JSON Definition

When Azure's 100+ built-in roles are too broad or too restrictive, you can craft a **Custom RBAC Role**:

```json
{
  "Name": "Data Pipeline Operator",
  "IsCustom": true,
  "Description": "Can trigger Azure Data Factory pipelines and read blob data without altering infrastructure",
  "Actions": [
    "Microsoft.DataFactory/factories/read",
    "Microsoft.DataFactory/factories/pipelines/read",
    "Microsoft.DataFactory/factories/pipelines/createRun/action",
    "Microsoft.Storage/storageAccounts/blobServices/containers/read",
    "Microsoft.Storage/storageAccounts/blobServices/generateUserDelegationKey/action"
  ],
  "NotActions": [],
  "DataActions": [
    "Microsoft.Storage/storageAccounts/blobServices/containers/blobs/read"
  ],
  "NotDataActions": [],
  "AssignableScopes": [
    "/subscriptions/11111111-2222-3333-4444-555555555555"
  ]
}
```

---

## ⚖️ 2. Actions vs. DataActions Explained

| Property | Plane | Checked By | Real-World Example |
| :--- | :--- | :--- | :--- |
| **`Actions`** | **Control Plane** | Azure Resource Manager (ARM) | Create or delete a storage account (`Microsoft.Storage/storageAccounts/write`). |
| **`DataActions`** | **Data Plane** | Individual Storage/DB Service | Read or write actual file contents inside the storage container (`Microsoft.Storage/.../blobs/read`). |

> ⚠️ **Critical Production Gotcha:** A user assigned the built-in **Contributor** role has full `Actions` but **ZERO `DataActions`**. A Contributor can create, modify, or delete a storage account, but **cannot read the data files inside it** unless explicitly granted a data role such as *Storage Blob Data Reader*!

---

### 💻 Creating and Assigning the Custom Role via CLI

```bash
# 1. Create the Custom Role from JSON definition
az role definition create --role-definition @custom-role.json

# 2. Assign the Custom Role to a Security Group on a Resource Group
az role assignment create \
  --assignee "b2c3d4e5-f6a7-8901-bcde-f12345678901" \
  --role "Data Pipeline Operator" \
  --scope "/subscriptions/<sub-id>/resourceGroups/rg-data-platform"
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 03 - Azure Role Based Access Control RBAC Architecture](./03-Azure-Role-Based-Access-Control-RBAC-Architecture.md) | [Index](../../../README.md) | [05 - Managed Identities System Assigned vs User Assigned →](./05-Managed-Identities-System-Assigned-vs-User-Assigned.md) |
