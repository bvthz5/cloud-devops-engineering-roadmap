# 06 - Azure Files, Azure File Sync & Network File Shares

> **Focus:** Fully managed shared file systems, SMB 3.0 vs. NFS 4.1 protocols, ISP Port 445 workarounds, Azure File Sync hybrid architectures, and persistent OS mount scripts.

---

## 📂 1. Azure Files Architecture

**Azure Files** provides fully managed cloud file shares accessible via standard industry network protocols (**SMB 3.0** and **NFS 4.1**). Unlike Azure Blob Storage (an object store accessed via HTTP REST APIs), Azure Files can be **mounted directly like a network drive** on Windows, Linux, and macOS.

```text
                  ┌───────────────────────────────────────┐
                  │        Azure Storage Account          │
                  │   ┌───────────────────────────────┐   │
                  │   │      File Share: /finance     │   │
                  │   │   ├── /spreadsheets           │   │
                  │   │   └── /invoices               │   │
                  │   └───────────────────────────────┘   │
                  └───────────────────────────────────────┘
                                ↑           ↑
             ┌──────────────────┘           └──────────────────┐
             │ SMB 3.0 (Port 445)                              │ NFS 4.1 (Port 2049)
    ┌────────────────┐                                ┌────────────────┐
    │   Windows VM   │                                │    Linux VM    │
    │   net use Z:   │                                │ mount -t nfs.. │
    └────────────────┘                                └────────────────┘
```

### 📊 SMB vs. NFS Comparison:

| Feature | Server Message Block (SMB 3.0) | Network File System (NFS 4.1) |
| :--- | :--- | :--- |
| **Supported OS** | Windows, Linux, macOS | **Linux Only** |
| **Default Network Port** | **Port 445** (TCP) | **Port 2049** (TCP) |
| **Authentication** | Active Directory, Entra ID Kerberos, Storage Key | Storage Key / Private VNet IP ACL |
| **Required Storage Tier**| Standard or Premium | **Premium Tier Only** |
| **ISP Port 445 Issue** | ⚠️ Many home/office ISPs block outbound port 445. Requires Azure VPN or ExpressRoute to mount on-premise. | Safe across private corporate networks and peered VNets. |

---

## 🔄 2. Azure File Sync (Hybrid Cloud File Server)

```text
On-Premises Branch File Server ◄────── Fast Sync ──────► Azure Cloud File Share
  (Acts as local low-latency cache)                          (Central master archive)
```

- **Cloud Tiering:** Automatically caches frequently accessed ("hot") files locally on-premises. Infrequently accessed ("cold") files are seamlessly tiered up to Azure Files, preserving expensive on-premises disk space.

---

### 💻 Mounting Azure File Shares (Windows & Linux)

```powershell
# Windows Persistent Mount (PowerShell)
cmdkey /add:mystorageacct.file.core.windows.net /user:Azure\mystorageacct /pass:<storage-key>
net use Z: \\mystorageacct.file.core.windows.net\finance /persistent:yes
```

```bash
# Linux SMB Mount
sudo apt update && sudo apt install -y cifs-utils
sudo mkdir -p /mnt/finance
sudo mount -t cifs //mystorageacct.file.core.windows.net/finance /mnt/finance \
  -o vers=3.0,username=mystorageacct,password=<storage-key>,serverino
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 05 - Storage Networking Private Endpoints and Firewalls](./05-Storage-Networking-Private-Endpoints-and-Firewalls.md) | [Index](../../../README.md) | [07 - Real World Scenarios →](./07-Real-World-Scenarios.md) |
