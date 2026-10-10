# 04 - Azure Storage Security: Account Keys, Shared Access Signatures (SAS) & User Delegation

> **Focus:** Deep dive into Storage Account access control, Access Keys rotation, Service SAS, Account SAS, User Delegation SAS (Entra ID integration), Stored Access Policies, and avoiding credential leakage.

---

## 🔐 1. What, Why, and History of Azure Storage Security

### What is Storage Security?
Azure Storage provides multiple authentication and authorization mechanisms to control who can read, write, list, or delete blobs, containers, file shares, tables, and queues.

### Why do We Need Multiple Tiers of Storage Access?
1. **Superuser Risk:** A raw Storage Account Key provides **unrestricted root administrator access** to all data in the storage account. Leaking this key in code repositories or client mobile apps causes catastrophic data breaches.
2. **Time-Limited Least Privilege:** External clients (e.g., mobile users uploading profile photos or web clients downloading invoices) need granular, temporary access to a single file without knowing master credentials.
3. **Auditability & Revocation:** Administrators need the ability to revoke leaked tokens instantly without rotating master keys and breaking other running production services.

### 📜 History & Evolution:
- **2010:** Azure Storage launches with dual 512-bit symmetric Access Keys (Key 1 and Key 2). Anyone with the key has total read/write/delete authority.
- **2012:** Shared Access Signatures (SAS) introduced, enabling time-bound URI query parameter tokens specifying exact permissions (Read, Write, List, Delete) and IP range restrictions.
- **2019:** **User Delegation SAS** released, allowing SAS tokens to be signed using Azure Entra ID (Azure AD) user/service credentials rather than the master account key. This prevents master key exposure entirely.
- **2021:** Microsoft introduces the capability to **disable Shared Key access completely** (`allowSharedKeyAccess = false`), enforcing strict Entra ID RBAC for all storage operations.

---

## 🔑 2. The 3 Types of Shared Access Signatures (SAS) Compared

```text
┌─────────────────────────────────────────────────────────────────────────────────────────┐
│                               AZURE SAS TOKEN ARCHITECTURE                              │
├─────────────────────────────────────────────────────────────────────────────────────────┤
│ https://myaccount.blob.core.windows.net/contracts/doc.pdf?<SAS-TOKEN-PARAMETERS>        │
│                                                                                         │
│   • sp=r               ➔ Permissions (r = read, w = write, d = delete, l = list)        │
│   • st=2026-10-10T12:00➔ Start Time (UTC)                                               │
│   • se=2026-10-10T14:00➔ Expiration Time (UTC - mandatory expiration)                   │
│   • spr=https          ➔ Allowed Protocol (HTTPS only)                                  │
│   • sip=203.0.113.0/24 ➔ IP Restriction (Allowed client subnet)                         │
│   • sig=a9x...         ➔ Cryptographic HMAC-SHA256 signature calculated with key       │
└─────────────────────────────────────────────────────────────────────────────────────────┘
```

| SAS Type | Signed With | Scope / Granularity | Revocation Method | Enterprise Recommendation |
| :--- | :--- | :--- | :--- | :--- |
| **User Delegation SAS** | **Azure Entra ID Token** (OAuth 2.0) | Blob container or specific blob only | Revoke Entra ID user session or delete User Delegation Key | **Best Practice** (Master keys are never exposed) |
| **Service SAS** | Storage Account Access Key | Single service (Blob, Queue, Table, File) | Requires Stored Access Policy on container to revoke | Use when Entra ID is not supported |
| **Account SAS** | Storage Account Access Key | Entire storage account across multiple services | **Cannot be revoked** without rotating Account Master Keys | **High Risk**; avoid in production client apps |

---

## 🔄 3. Account Key Rotation Strategy & Zero Downtime

Every Azure Storage account contains two 512-bit symmetric keys: **Key 1** and **Key 2**. Having two keys enables zero-downtime key rotation:

```text
Step 1: Application currently authenticates using Key 1.
Step 2: Update application configuration / Azure Key Vault secret to use Key 2.
Step 3: Restart application pods; verify traffic flows seamlessly on Key 2.
Step 4: Regenerate Key 1 via Azure CLI:
        az storage account keys renew --account-name mystore --key primary
Step 5: Next rotation cycle, switch application to newly regenerated Key 1, then renew Key 2.
```

---

## 🛡️ 4. Stored Access Policies: Instant SAS Revocation

A fundamental limitation of ad-hoc Service SAS tokens is that once generated, they cannot be revoked until they naturally expire. A **Stored Access Policy** solves this:

1. A policy is created on the container level defining permissions and expiration:
   ```bash
   az storage container policy create \
     --account-name storprod \
     --container-name invoices \
     --name PolicyInvoiceQ4 \
     --permissions r \
     --expiry 2026-12-31T23:59:59Z
   ```
2. The SAS token is generated referencing the policy identifier (`si=PolicyInvoiceQ4`):
   ```bash
   az storage blob generate-sas \
     --account-name storprod \
     --container-name invoices \
     --name invoice_9921.pdf \
     --policy-name PolicyInvoiceQ4 \
     --full-uri
   ```
3. **Instant Revocation:** If a token is compromised, modifying or deleting `PolicyInvoiceQ4` immediately invalidates all SAS tokens generated from it without rotating the master account key!

---

## 💻 5. Best Practice: Completely Disabling Shared Key Access

To achieve absolute compliance (SOC2, PCI-DSS, ISO 27001), enterprises disable shared keys entirely, forcing 100% of applications to authenticate via Entra ID Managed Identities:

```bash
# Disable Shared Key access on Storage Account
az storage account update \
  --name storsecureprod \
  --resource-group rg-storage \
  --allow-shared-key-access false
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 03 - Blob Access Tiers Hot Cool Cold Archive](./03-Blob-Access-Tiers-Hot-Cool-Cold-Archive.md) | [Index](../../../README.md) | [05 - Storage Networking Private Endpoints and Firewalls →](./05-Storage-Networking-Private-Endpoints-and-Firewalls.md) |
