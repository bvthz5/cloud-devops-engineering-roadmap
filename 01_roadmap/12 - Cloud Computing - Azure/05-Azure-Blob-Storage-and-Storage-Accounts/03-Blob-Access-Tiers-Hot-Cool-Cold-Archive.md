# 03 - Blob Access Tiers: Hot, Cool, Cold, Archive & Lifecycle Automation

> **Focus:** Economic optimization, access frequency trade-offs, minimum retention requirements, Archive rehydration latency (1–15 hours), and automated Lifecycle Management policy JSON rules.

---

## 🧊 1. Blob Access Tiers Spectrum

Azure Blob Storage provides 4 distinct storage tiers that balance storage costs against access fees:

```text
Access Frequency: HIGH ◄────────────────────────────────────────────────► LOW
Storage Cost:     HIGH ◄────────────────────────────────────────────────► LOWEST
Access Cost:      LOW  ◄────────────────────────────────────────────────► HIGHEST

┌─────────────────┐    ┌─────────────────┐    ┌─────────────────┐    ┌─────────────────┐
│       HOT       │    │      COOL       │    │      COLD       │    │     ARCHIVE     │
│                 │    │                 │    │                 │    │                 │
│ Active data,    │    │ Infrequent      │    │ Rarely accessed │    │ Long-term       │
│ daily access.   │    │ monthly access. │    │ (90+ days).     │    │ compliance.     │
│ Instant access. │    │ Instant access. │    │ Instant access. │    │ Rehydrate first │
│ No min duration │    │ 30-day min.     │    │ 90-day min.     │    │ 180-day min.    │
└─────────────────┘    └─────────────────┘    └─────────────────┘    └─────────────────┘
```

### 📊 Comprehensive Access Tiers Matrix:

| Tier | Retrieval Latency | Minimum Storage Duration | Early Deletion Fee? | Enterprise Use Cases |
| :--- | :---: | :---: | :---: | :--- |
| **Hot** | Milliseconds | None | ❌ No | Active user data, live website images, transactional file processing. |
| **Cool** | Milliseconds | **30 Days** | ✅ Yes (Pro-rated if deleted < 30 days) | Short-term backups, customer invoices viewed occasionally. |
| **Cold** | Milliseconds | **90 Days** | ✅ Yes (Pro-rated if deleted < 90 days) | Historical logs, quarterly reporting archives, audit logs. |
| **Archive** | **1 to 15 Hours** | **180 Days** | ✅ Yes (Pro-rated if deleted < 180 days) | Healthcare records, legal litigation holds, compliance backups. |

---

## ⚡ 2. Archive Tier Rehydration Mechanics

Data in the **Archive Tier** is stored offline on magnetic tape and cold disks. **You cannot read an archived blob directly.** To view or download an archived blob, you must **rehydrate** it back to Hot or Cool:

```text
┌─────────────────┐                             ┌─────────────────┐
│  Archived Blob  │ ──(Rehydration: 1-15 hrs)──►│ Hot / Cool Blob │ ──► Available to Apps
└─────────────────┘                             └─────────────────┘
```

### Two Rehydration Strategies:
1. **Change Tier In-Place:** Change the tier of the archived blob directly to Hot or Cool (`az storage blob set-tier --tier Hot`). The blob remains offline until rehydration completes.
2. **Copy to New Blob (Recommended):** Use `az storage blob copy start` with `--destination-tier Hot`. Your archived blob remains safely archived while an active working copy becomes available in Hot tier.

---

## 📜 3. Automated Lifecycle Management Policy

Avoid manual tier management by creating automated JSON lifecycle policies evaluated daily by Azure:

```json
{
  "rules": [
    {
      "name": "auto-archive-and-delete-pipeline",
      "enabled": true,
      "type": "Lifecycle",
      "definition": {
        "filters": {
          "blobTypes": ["blockBlob"],
          "prefixMatch": ["raw-logs/"]
        },
        "actions": {
          "baseBlob": {
            "tierToCool":    { "daysAfterModificationGreaterThan": 30 },
            "tierToCold":    { "daysAfterModificationGreaterThan": 90 },
            "tierToArchive": { "daysAfterModificationGreaterThan": 180 },
            "delete":        { "daysAfterModificationGreaterThan": 730 }
          },
          "snapshot": {
            "delete": { "daysAfterCreationGreaterThan": 90 }
          }
        }
      }
    }
  ]
}
```

```bash
# Apply Lifecycle Policy via Azure CLI
az storage account management-policy create   --account-name mystorageacctprod   --resource-group rg-storage   --policy @lifecycle-policy.json
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 02 - Azure Blob Storage Architecture and Blob Types](./02-Azure-Blob-Storage-Architecture-and-Blob-Types.md) | [Index](../../../README.md) | [04 - Storage Security Shared Access Signatures SAS and Keys →](./04-Storage-Security-Shared-Access-Signatures-SAS-and-Keys.md) |
