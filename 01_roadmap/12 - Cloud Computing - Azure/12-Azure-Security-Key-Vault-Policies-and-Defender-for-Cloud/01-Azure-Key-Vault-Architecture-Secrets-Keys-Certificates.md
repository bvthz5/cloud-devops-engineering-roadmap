# 01 - Azure Key Vault Architecture: Secrets, Keys, Certificates & HSM Protection

> **Focus:** Enterprise secret and cryptographic key management in Azure. Deep dive into the 3 core object types (Secrets, Keys, Certificates), Standard vs. Premium Managed HSM, Soft Delete & Purge Protection, Envelope Encryption with Customer-Managed Keys (CMK), and automated TLS certificate rotation.

---

## 🔐 1. What, Why, and History of Azure Key Vault

### What is Azure Key Vault?
Azure Key Vault is a cloud-hosted, highly secure, Hardware Security Module (HSM)-backed management service for centrally storing, accessing, and lifecycle-managing application secrets, cryptographic keys, and TLS/SSL certificates.

### Why do Modern Cloud Architectures Mandate Key Vault?
1. **Eliminate Hardcoded Secrets:** Prevents database passwords, third-party API tokens, and connection strings from ever being committed to Git repositories or exposed in container image environment variables.
2. **Centralized Auditing & Access Control:** Every single read, decrypt, or sign operation generates an audit log record in Log Analytics detailing the caller's Entra ID identity, IP address, and timestamp.
3. **Hardware Security Compliance (FIPS 140):** Meets stringent regulatory requirements (PCI-DSS, FedRAMP, HIPAA, SOC 2) by executing cryptographic operations inside physical FIPS 140-2 Level 2 and Level 3 validated HSMs.

### 📜 History & Evolution:
- **2015:** Azure Key Vault launches in General Availability with software-protected secrets and HSM-protected keys.
- **2020:** **Soft Delete** enabled by default across all Key Vaults to prevent accidental loss.
- **2021:** **Azure Key Vault Managed HSM** introduced, offering single-tenant, fully managed, FIPS 140-2 Level 3 cryptographic hardware for ultra-high-throughput enterprise banking workloads.

---

## 🏛️ 2. The 3 Core Key Vault Object Types

```text
┌────────────────────────────────────────────────────────────────────────────────────────┐
│                              AZURE KEY VAULT OBJECT TYPES                              │
├────────────────────────────────────────────────────────────────────────────────────────┤
│ 1. SECRETS (Raw Sensitive Strings)                                                     │
│    • Passwords, database connection strings, API tokens, private keys.                 │
│    • Maximum size: Up to 25 KB per secret.                                             │
│    • Retrieved directly as plain text in-memory by authorized applications.            │
│                                                                                        │
│ 2. KEYS (Cryptographic Keys - RSA / Elliptic Curve)                                    │
│    • Used for Encryption, Decryption, Digital Signatures, and Verification.            │
│    • **THE PRIVATE KEY NEVER LEAVES THE VAULT/HSM HARDWARE!**                          │
│    • Applications submit data to Key Vault; Key Vault performs cryptographic math      │
│      internally and returns the encrypted/decrypted result.                            │
│                                                                                        │
│ 3. CERTIFICATES (X.509 TLS/SSL Certificates)                                           │
│    • Manages public/private key pairs and X.509 metadata for website HTTPS.            │
│    • Automated enrollment and lifecycle rotation with integrated Certificate           │
│      Authorities (DigiCert, GlobalSign) or custom private enterprise CAs.              │
└────────────────────────────────────────────────────────────────────────────────────────┘
```

---

## 🛡️ 3. Critical Protection Mechanisms: Soft Delete & Purge Protection

Accidental deletion of a production Key Vault by a compromised admin account can destroy an entire enterprise's encrypted data permanently. Microsoft enforces two critical layers:

```text
┌────────────────────────────────────────────────────────────────────────────────────────┐
│                              KEY VAULT LIFECYCLE DEFENSE                               │
├────────────────────────────────────────────────────────────────────────────────────────┤
│ 1. SOFT DELETE (Mandatory & Non-Disableable):                                          │
│    • When a Key Vault or secret is deleted, it is NOT destroyed immediately.           │
│    • Moved to a "Soft-Deleted" state for a retention period (default 90 days).         │
│    • Can be recovered instantly with all secrets intact: az keyvault recover.          │
│                                                                                        │
│ 2. PURGE PROTECTION (Must be Explicitly Enabled for Production):                       │
│    • Enforces a mandatory retention window where NO ONE—NOT EVEN THE SUBSCRIPTION      │
│      GLOBAL ADMIN OR MICROSOFT SUPPORT—can permanently purge/delete the vault!         │
│    • Completely defeats ransomware attacks attempting to delete cryptographic keys!    │
└────────────────────────────────────────────────────────────────────────────────────────┘
```

---

## ⚖️ 4. Standard Key Vault vs. Premium Managed HSM

| Feature | Standard Key Vault | Premium Key Vault | Managed HSM (Dedicated) |
| :--- | :--- | :--- | :--- |
| **Tenant Model** | Multi-tenant shared infrastructure | Multi-tenant shared infrastructure | **Single-tenant dedicated HSM pool** |
| **Hardware Compliance** | FIPS 140-2 Level 2 (Software Keys) | FIPS 140-2 Level 2 (HSM Keys) | **FIPS 140-2 Level 3** |
| **Access Control Model** | Access Policies & Azure RBAC | Access Policies & Azure RBAC | Local RBAC (Independent from ARM) |
| **Throughput / Scale** | Standard regional rate limits | Standard regional rate limits | High-throughput (Thousands of ops/sec)|
| **Cost Profile** | Pay per transaction (Fraction of cent)| Pay per transaction + HSM key fee | Fixed hourly cost per pool (~$4.50/hr)|

---

## 💻 5. Practical CLI: Deploying a Hardened Key Vault

```bash
# 1. Create a Hardened Production Key Vault with Purge Protection
az keyvault create \
  --name "kv-corp-prod-eastus" \
  --resource-group "rg-security-prod" \
  --location eastus \
  --sku standard \
  --enable-purge-protection true \
  --retention-days 90 \
  --enable-rbac-authorization true

# 2. Store a sensitive database connection string
az keyvault secret set \
  --vault-name "kv-corp-prod-eastus" \
  --name "DbConnectionString" \
  --value "Server=tcp:sql-prod.database.windows.net;Database=banking;User ID=appadmin;Password=Secret123!"

# 3. Retrieve secret securely
az keyvault secret show \
  --vault-name "kv-corp-prod-eastus" \
  --name "DbConnectionString" \
  --query value -o tsv
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← Module Overview](./00-Overview.md) | [Index](../../../README.md) | [02 - Key Vault Access Policies vs Azure RBAC Control →](./02-Key-Vault-Access-Policies-vs-Azure-RBAC-Control.md) |
