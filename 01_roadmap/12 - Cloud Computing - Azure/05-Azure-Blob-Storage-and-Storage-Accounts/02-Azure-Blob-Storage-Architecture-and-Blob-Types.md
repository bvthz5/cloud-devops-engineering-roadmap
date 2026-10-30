# 02 - Azure Blob Storage Architecture, Blob Types & Security Models

> **Focus:** Object storage hierarchy, Block Blobs vs. Append Blobs vs. Page Blobs, Container access policies, and Enterprise authentication (Storage Keys, SAS Tokens, Entra ID RBAC).

---

## 🏛️ 1. Azure Blob Storage Hierarchy

Azure Blob Storage provides massively scalable, cost-effective object storage designed for unstructured data:

```text
Storage Account (Global DNS Namespace: https://<account>.blob.core.windows.net)
   └── Container (Logical grouping / folder / bucket equivalent)
        └── Blob (The actual binary object)
             ├── Block Blob   (Documents, video media, data backups)
             ├── Append Blob  (Write-once, append-only application logs)
             └── Page Blob    (512-byte random I/O for Virtual Hard Disks)
```

---

## 🧩 2. The Three Blob Types Explained

| Blob Type | Internal Structure | Maximum Size | Enterprise Use Case |
| :--- | :--- | :---: | :--- |
| **Block Blob** | Composed of discrete blocks (up to `50,000` blocks of up to `4,000 MiB` each). | **~190.7 TB** | Web files, photos, video streaming, database backups, and ADLS Gen2 data lakes. |
| **Append Blob** | Optimized strictly for append operations. Existing data cannot be edited or deleted; new blocks are simply appended to the end. | **195 GB** | Real-time log aggregation, telemetry streams, audit tracking. |
| **Page Blob** | Collection of 512-byte pages designed for continuous random read and write operations. | **8 TB** | Virtual Hard Disks (VHDs) backing Azure IaaS Virtual Machines. |

---

## 🔐 3. Storage Security & Authentication Layers

Azure provides 4 distinct security mechanisms to access storage resources:

| Security Method | Scope & Mechanics | Security Level | Best Enterprise Use Case |
| :--- | :--- | :---: | :--- |
| **Entra ID RBAC** | Identity-based. Uses OAuth 2.0 tokens from users, service principals, or Managed Identities. | 🟢 **Highest (Zero-Trust)** | Production applications, CI/CD pipelines, internal staff access. No passwords stored in code. |
| **SAS Token** | Signed URI granting scoped, time-limited permissions (e.g., Read only for 1 hour). | 🟡 **High** | Sharing assets securely with external partners or client web uploads. |
| **Storage Account Key** | Full super-admin root key. Bypasses all RBAC permissions. | 🔴 **High Risk** | Initial platform automation scripts only; rotate frequently via Key Vault. |
| **Anonymous Access** | Public read access configured at container or blob level. | ⚪ **Public** | Static public website media, publicly distributed installer packages. |

---

### 💻 Azure CLI Blob Management & SAS Token Generation

```bash
# 1. Create a Private Container inside Storage Account
az storage container create   --name documents   --account-name mystorageacctprod   --auth-mode login

# 2. Upload a File as a Block Blob
az storage blob upload   --container-name documents   --name "reports/2025/q1_financials.pdf"   --file ./q1_financials.pdf   --account-name mystorageacctprod   --auth-mode login

# 3. Generate a Scoped SAS Token (Read-Only for 60 minutes)
az storage blob generate-sas   --container-name documents   --name "reports/2025/q1_financials.pdf"   --account-name mystorageacctprod   --permissions r   --expiry $(date -u -d "1 hour" '+%Y-%m-%dT%H:%MZ')   --full-uri   --output tsv
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 01 - Azure Storage Account Types and Redundancy](./01-Azure-Storage-Account-Types-and-Redundancy.md) | [Index](../../../README.md) | [03 - Blob Access Tiers Hot Cool Cold Archive →](./03-Blob-Access-Tiers-Hot-Cool-Cold-Archive.md) |
