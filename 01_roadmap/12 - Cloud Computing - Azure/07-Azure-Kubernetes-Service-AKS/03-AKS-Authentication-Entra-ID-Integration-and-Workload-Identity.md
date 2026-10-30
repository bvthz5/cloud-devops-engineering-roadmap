# 03 - AKS Security: Microsoft Entra ID Integration & Workload Identity

> **Focus:** Kubernetes authentication, Entra ID RBAC mappings, Azure Workload Identity (Federated OIDC), and passwordless pod access.

---

## 🔐 1. Azure Workload Identity Architecture

**Microsoft Entra Workload Identity** replaces the legacy AAD Pod Identity. It allows Kubernetes pods to authenticate directly with Azure services using **OpenID Connect (OIDC)** without hardcoding secrets:

```text
┌───────────────────────────────┐
│     Kubernetes Pod (KSA)      │
│  (ServiceAccount: sa-backend) │
└──────────────┬────────────────┘
               │ 1. Mounts projected Kubernetes Service Account token
               ▼
┌───────────────────────────────┐
│      AKS OIDC Discovery       │
│     (Federated Identity)      │
└──────────────┬────────────────┘
               │ 2. Validates token with Microsoft Entra ID
               ▼
┌───────────────────────────────┐
│ Microsoft Entra ID (User MI)  │
│ (Issues OAuth 2.0 Token)      │
└──────────────┬────────────────┘
               ▼
┌───────────────────────────────┐
│  Azure Key Vault / Azure SQL  │
└───────────────────────────────┘
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 02 - AKS Networking Kubenet vs Azure CNI](./02-AKS-Networking-Kubenet-vs-Azure-CNI.md) | [Index](../../../README.md) | [04 - Azure Container Registry ACR and AKS Integration →](./04-Azure-Container-Registry-ACR-and-AKS-Integration.md) |
