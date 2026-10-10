# 05 - Azure Managed Identities: System-Assigned vs. User-Assigned

> **Focus:** Eliminating hardcoded secrets in source code, Azure Instance Metadata Service (IMDS), System-Assigned vs. User-Assigned comparison, and Python Azure SDK passwordless access.

---

## 🤖 1. What is a Managed Identity?

A **Managed Identity** provides an automatically managed identity in Microsoft Entra ID for Azure resources to use when connecting to services that support Entra ID authentication (such as Azure Key Vault, Azure SQL, and Azure Storage).

```text
┌──────────────────────────────────────┐
│          Azure Virtual Machine       │
│  (Zero passwords in code or config!) │
└──────────────────┬───────────────────┘
                   │ 1. Requests token from local Azure Instance Metadata Service (IMDS)
                   ▼
┌──────────────────────────────────────┐
│        Microsoft Entra ID (AAD)      │
│  (Validates identity of the VM)      │
└──────────────────┬───────────────────┘
                   │ 2. Issues short-lived OAuth 2.0 access token
                   ▼
┌──────────────────────────────────────┐
│           Azure Key Vault            │
│  (Grants access based on Azure RBAC) │
└──────────────────────────────────────┘
```

---

## 📊 2. System-Assigned vs. User-Assigned Comparison

| Architectural Feature | System-Assigned Managed Identity | User-Assigned Managed Identity |
| :--- | :--- | :--- |
| **Creation & Lifecycle** | Enabled directly on an Azure resource. **Tied 1:1 to resource lifecycle**. Deleting the VM automatically deletes the identity. | Created as an independent standalone Azure resource. Lifecyle is managed independently. |
| **Sharing** | **Cannot be shared**. Exactly one resource per identity. | **Can be shared** across multiple VMs, container apps, and functions. |
| **Typical Use Case** | Workloads running on a single dedicated VM or App Service. | Clustered workloads (e.g., 10 VMSS instances sharing access to one Key Vault). |

---

### 💻 Python Code: Passwordless Authentication via Managed Identity

```python
from azure.identity import DefaultAzureCredential
from azure.keyvault.secrets import SecretClient

# DefaultAzureCredential automatically searches for Managed Identity on Azure VMs!
credential = DefaultAzureCredential()
client = SecretClient(vault_url="https://my-keyvault.vault.azure.net/", credential=credential)

# Fetch database password securely without hardcoding any API keys
db_secret = client.get_secret("database-prod-password")
print(f"Secret retrieved securely: {db_secret.value}")
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 04 - Creating Custom RBAC Roles JSON Definitions](./04-Creating-Custom-RBAC-Roles-JSON-Definitions.md) | [Index](../../../README.md) | [06 - Service Principals App Registrations and PIM →](./06-Service-Principals-App-Registrations-and-PIM.md) |
