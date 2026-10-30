# 02 - Microsoft Entra ID: Users, Groups, Dynamic Membership & Administrative Units

> **Focus:** Cloud Identity management, User types (Cloud, Synced, Guest B2B), Security Groups vs. M365 Groups, Dynamic Membership rules, and Administrative Units (AUs) for scoped administration.

---

## 👥 1. Microsoft Entra ID User Types

Every identity inside an Entra ID tenant belongs to one of three primary categories:

| User Type | Origin & Authentication | Typical Enterprise Use Case |
| :--- | :--- | :--- |
| **Cloud-Only User** | Created directly in the Entra ID cloud portal or via Microsoft Graph API. | Cloud-native employees, DevOps service engineers, cloud administrators. |
| **Directory-Synced User**| Synchronized from on-premises Active Directory Domain Services (AD DS) via **Microsoft Entra Connect**. | Corporate staff who authenticate using their on-premises domain passwords. |
| **Guest / External User (B2B)**| Invited from an external Entra ID tenant or personal email (Google, Outlook). | External consultants, auditors, contractors, supply chain partners. |

---

## 👥 2. Security Groups vs. Microsoft 365 Groups

```text
┌──────────────────────────────────────┐       ┌──────────────────────────────────────┐
│           Security Groups            │       │         Microsoft 365 Groups         │
│                                      │       │                                      │
│  • Used exclusively to assign RBAC   │       │  • Collaboration engine              │
│    permissions and software licenses │       │  • Provisions a shared Outlook Inbox,│
│  • Can contain Users, Devices, and   │       │    SharePoint site, OneNote, and     │
│    Service Principals                │       │    Teams channel                     │
│  • Best for IT Governance            │       │  • Best for project teamwork         │
└──────────────────────────────────────┘       └──────────────────────────────────────┘
```

### ⚡ Assigned vs. Dynamic Group Membership:
- **Assigned Membership:** Administrators manually add and remove specific users.
- **Dynamic User Membership:** Users are automatically added or removed in real time based on user profile attributes evaluated by Entra ID:
  ```text
  # Dynamic Membership Rule Example:
  (user.department -eq "Engineering") and (user.country -eq "United States")
  ```

---

## 🏛️ 3. Administrative Units (AUs) for Scoped Delegation

In large multinational enterprises, giving a regional IT administrator the global **User Administrator** role allows them to accidentally reset the CEO's password or modify executive accounts worldwide!

```text
Entra ID Tenant (Global Scope)
├── Administrative Unit: European Operations
│   ├── All German & French Staff Users
│   └── Regional IT Admin assigned User Administrator strictly at AU Scope!
└── Administrative Unit: APAC Operations
    ├── All Australian & Indian Staff Users
    └── Regional IT Admin assigned User Administrator strictly at AU Scope!
```

- **Administrative Units (AUs)** let you subdivide your organization into granular administrative boundaries. Regional administrators get elevated privileges **strictly over the users and groups inside their designated AU**.

---

### 💻 Azure CLI Commands for Groups & Users

```bash
# 1. Create a Security Group
az ad group create \
  --display-name "Data-Platform-Engineers" \
  --mail-nickname "data-engineers"

# 2. Add a User to the Group
az ad group member add \
  --group "Data-Platform-Engineers" \
  --member-id "a1b2c3d4-e5f6-7890-abcd-ef1234567890"

# 3. Create an Administrative Unit
az rest --method post \
  --url "https://graph.microsoft.com/v1.0/directory/administrativeUnits" \
  --body '{"displayName":"EMEA-Staff","description":"Administrative Unit for Europe, Middle East, Africa"}'
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 01 - Microsoft Entra ID Architecture vs Active Directory](./01-Microsoft-Entra-ID-Architecture-vs-Active-Directory.md) | [Index](../../../README.md) | [03 - Azure Role Based Access Control RBAC Architecture →](./03-Azure-Role-Based-Access-Control-RBAC-Architecture.md) |
