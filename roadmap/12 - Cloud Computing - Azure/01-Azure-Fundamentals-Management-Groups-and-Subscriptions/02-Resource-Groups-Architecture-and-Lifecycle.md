# 02 - Azure Resource Groups: Lifecycle, Scoping, Tagging & Locks

> **Focus:** Logical resource organization, Resource Group lifecycle boundaries, metadata tagging strategies, and Resource Locks (`CanNotDelete` vs. `ReadOnly`).

---

## 📁 1. Resource Group Architecture

A **Resource Group (RG)** is a logical container that holds related Azure resources for an application solution. 

```text
┌────────────────────────────────────────────────────────┐
│             RESOURCE GROUP: rg-ecommerce-prod          │
│                                                        │
│  All resources share the same lifecycle:               │
│  ├── Virtual Network (vnet-prod)                       │
│  ├── Virtual Machines (vm-web-01, vm-web-02)           │
│  ├── Storage Account (stprodassets)                    │
│  └── Key Vault (kv-prod-secrets)                       │
│                                                        │
│  • Deleting the Resource Group deletes EVERYTHING!     │
│  • RBAC and Azure Policy applied at RG level inherit   │
│  • Resource Group location stores METADATA, not data   │
└────────────────────────────────────────────────────────┘
```

### 📌 Core Rules of Resource Groups:
1. **Single Group Membership:** Every Azure resource must exist in exactly one resource group.
2. **Cross-Region Placement:** Resources inside a resource group do **not** need to reside in the same region as the resource group. The RG location merely dictates where management control plane metadata is stored.
3. **Unified Lifecycle:** Resources deployed, updated, or decommissioned together should live in the same resource group.
4. **Moving Resources:** Resources can be moved to another resource group or subscription without downtime for supported resource types.

---

## 🔒 2. Resource Locks (`CanNotDelete` vs. `ReadOnly`)

Resource locks prevent accidental deletion or modification of critical production infrastructure:

| Lock Type | CLI Parameter | What It Allows | What It Blocks |
| :--- | :--- | :--- | :--- |
| **CanNotDelete** | `--lock-type CanNotDelete` | Authorized users can read and modify resources. | **Blocks all deletions!** Prevents catastrophic accidental drop. |
| **ReadOnly** | `--lock-type ReadOnly` | Authorized users can only read/view resources. | **Blocks all changes and deletions!** (Similar to restricting Contributor to Reader). |

```bash
# Apply a CanNotDelete lock to a production Resource Group
az lock create \
  --name "PreventAccidentalDrop" \
  --resource-group rg-ecommerce-prod \
  --lock-type CanNotDelete \
  --notes "Production lock: must be removed by Cloud Governance lead to delete"
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 01 - Azure Resource Hierarchy Tenant Management Groups Subscriptions Resource Groups](./01-Azure-Resource-Hierarchy-Tenant-Management-Groups-Subscriptions-Resource-Groups.md) | [Index](../../../README.md) | [03 - Azure Subscriptions Types Limits and Quotas →](./03-Azure-Subscriptions-Types-Limits-and-Quotas.md) |
