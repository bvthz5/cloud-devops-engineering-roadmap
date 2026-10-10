# 06 - Service Principals, App Registrations & Privileged Identity Management (PIM)

> **Focus:** Automated non-human identities, App Registrations vs. Service Principals, Client Secrets & Certificates, and Just-In-Time (JIT) access elevation with Microsoft Entra PIM.

---

## 🤖 1. App Registration vs. Service Principal

```text
┌──────────────────────────────────────┐       ┌──────────────────────────────────────┐
│           App Registration           │       │           Service Principal          │
│                                      │       │                                      │
│  • The GLOBAL template definition    │ ────► │  • The LOCAL instance of the app in  │
│    of your application               │ Inst. │    a specific Entra ID tenant        │
│  • Defines API permissions, Redirect │       │  • This is what you assign Azure     │
│    URIs, and Application ID (Client) │       │    RBAC permissions to!              │
└──────────────────────────────────────┘       └──────────────────────────────────────┘
```

---

## ⏱️ 2. Privileged Identity Management (PIM) for Just-In-Time Access

In a secure enterprise, **no engineer should have standing permanent Contributor or Owner access to Production**:

```text
Normal Everyday State:
  DevOps Engineer "Alice" has only READER role assigned.

When an Emergency Production Hotfix is Required:
  1. Alice logs into Azure Portal ➔ Privileged Identity Management (PIM).
  2. Alice activates the eligible "Contributor" role for 4 hours.
  3. Provides a mandatory business justification ticket number (#INC-9021).
  4. Approver receives an instant alert and grants permission.
  5. Contributor role activates and automatically EXPIRES in 4 hours!
```

---

### 💻 Creating a Service Principal for CI/CD Pipelines

```bash
# Create a Service Principal with Contributor role on a Resource Group
az ad sp create-for-rbac \
  --name "github-actions-deployer" \
  --role "Contributor" \
  --scopes "/subscriptions/<sub-id>/resourceGroups/rg-ecommerce-prod" \
  --sdk-auth
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 05 - Managed Identities System Assigned vs User Assigned](./05-Managed-Identities-System-Assigned-vs-User-Assigned.md) | [Index](../../../README.md) | [07 - Real World Scenarios →](./07-Real-World-Scenarios.md) |
