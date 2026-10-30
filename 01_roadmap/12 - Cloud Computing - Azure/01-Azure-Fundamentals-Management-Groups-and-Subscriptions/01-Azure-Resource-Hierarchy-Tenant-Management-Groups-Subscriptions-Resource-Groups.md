# 01 - Azure Resource Hierarchy: Tenant, Management Groups, Subscriptions, & Resource Groups

Azure manages millions of cloud resources across global enterprises by organizing them into a strictly structured, top-down governance hierarchy. Every resource—from a single virtual machine disk to an entire Kubernetes cluster—lives within this hierarchy. Understanding this 4-tier model is fundamental to implementing enterprise security, role-based access control (RBAC), Azure Policy compliance, and accurate billing attribution.

---

## 🏛️ 1. The 4-Tier Azure Resource Hierarchy

```text
┌────────────────────────────────────────────────────────────────────────────────────────┐
│                              AZURE RESOURCE HIERARCHY                                  │
│                                                                                        │
│   TIER 1: MICROSOFT ENTRA ID TENANT & ROOT MANAGEMENT GROUP                            │
│   (contoso.onmicrosoft.com - Absolute identity & organizational root)                  │
│                               │                                                        │
│                               ▼                                                        │
│   TIER 2: MANAGEMENT GROUPS (Governance & Policy Boundary)                             │
│   ┌───────────────────────────────────┬───────────────────────────────────┐            │
│   │ MG: Corporate Platform            │ MG: Non-Production                │            │
│   │ └── MG: Production                │ ├── MG: Development               │            │
│   │     └── MG: EMEA-Prod             │ └── MG: Staging                   │            │
│   └─────────────────┬─────────────────┴─────────────────┬─────────────────┘            │
│                     │                                   │                              │
│                     ▼                                   ▼                              │
│   TIER 3: SUBSCRIPTIONS (Billing & Trust Boundary)                                     │
│   ┌───────────────────────────────────┐ ┌─────────────────────────────────┐            │
│   │ Subscription: Prod-Core-Workloads │ │ Subscription: Dev-Sandboxes     │            │
│   │ (Invoice #1001 - Full EA Pricing) │ │ (Invoice #1002 - Dev/Test Rates)│            │
│   └─────────────────┬─────────────────┘ └─────────────────┬───────────────┘            │
│                     │                                     │                            │
│                     ▼                                     ▼                            │
│   TIER 4: RESOURCE GROUPS (Lifecycle Boundary)                                         │
│   ┌───────────────────────────────────┐ ┌─────────────────────────────────┐            │
│   │ Resource Group: rg-web-prod-eastus│ │ Resource Group: rg-data-prod-eu │            │
│   │ Tags: [env:prod, cost-center:402] │ │ Tags: [env:prod, data-lake:raw] │            │
│   └─────────────────┬─────────────────┘ └─────────────────┬───────────────┘            │
│                     │                                     │                            │
│                     ▼                                     ▼                            │
│   RESOURCES: Virtual Machines, VNets, Storage Accounts, Key Vaults, SQL Databases      │
└────────────────────────────────────────────────────────────────────────────────────────┘
```

---

## 🔬 2. Deep Dive: The 4 Governance Levels

### 1. Management Groups (Governance & Policy at Scale)
Management Groups provide a governance scope above subscriptions. If an organization has dozens or hundreds of Azure subscriptions, managing access and compliance individually on each subscription becomes unmanageable.

- **Key Architectural Rules:**
  - **Tree Depth:** Up to 6 levels of hierarchy depth (not counting the Root Management Group).
  - **Single Parent:** A subscription can belong to **only one** management group at any given time.
  - **Inheritance:** Policies, compliance initiatives, and RBAC role assignments applied at a management group automatically inherit downward to all child management groups, subscriptions, resource groups, and individual resources.
  - **Tenant Root Group:** Created automatically when your organization signs up; all subscriptions belong to this root tree by default.

#### Real-World Example: Applying an Azure Policy at Management Group Level
```bash
# Restrict ALL subscriptions under Production Management Group to East US only
az policy assignment create \
  --name "restrict-location-prod" \
  --policy "e56962a6-4747-49cd-b67b-bf8b01975c4f" \
  --scope "/providers/Microsoft.Management/managementGroups/Production" \
  --params '{"listOfAllowedLocations": {"value": ["eastus"]}}'
```
*Result:* Every subscription and resource group under the `Production` Management Group is permanently locked to East US; subscription owners cannot bypass this constraint.

---

### 2. Azure Subscriptions (Billing & Trust Boundary)
A Subscription is the fundamental **billing and quota container** in Azure. Every resource must reside inside exactly one subscription.

- **What a Subscription Controls:**
  1. **Billing Unit:** Generates an independent monthly invoice.
  2. **Trust Boundary:** Trust relationship anchored to exactly one Microsoft Entra ID (Azure AD) tenant.
  3. **Quota Limits:** Regional capacity caps (e.g., maximum 25,000 vCPUs per region, 1,000 VNets, 980 Resource Groups per subscription).

#### Subscription Types
| Subscription Type | Target Audience | Primary Characteristics |
| :--- | :--- | :--- |
| **Pay-As-You-Go** | Individuals / Startups | Standard pricing, credit card billing, no upfront commitment. |
| **Enterprise Agreement (EA)** | Large Enterprises | Volume discounts, enterprise credit balance, centralized billing. |
| **Dev/Test Subscription** | Development Teams | Heavily discounted pricing on Windows VMs and SQL; zero production SLA. |
| **Free Trial** | New Users | $200 free credit for 30 days + 12 months of popular free services. |
| **CSP (Cloud Solution Provider)** | Partner-Managed | Managed and invoiced directly by an authorized Microsoft partner. |

#### Why Multi-Subscription Architecture is an Enterprise Standard
- **Blast Radius Isolation:** A misconfiguration, security breach, or runaway script in `Dev` cannot exhaust quotas or disrupt live customer traffic in `Production`.
- **Cost Attribution:** Invoices cleanly align with distinct department cost centers (e.g., Data Engineering vs. Marketing).
- **Scale Beyond Quotas:** Prevents hitting regional subscription limits as fleets expand to tens of thousands of compute cores.

---

### 3. Resource Groups (Lifecycle & Logical Grouping)
A Resource Group (RG) is a logical container that holds related Azure resources that share the same lifecycle: deploy together, update together, and delete together.

- **Key Rules of Resource Groups:**
  - **Exclusive Membership:** A resource can belong to **only one** resource group at a time.
  - **Geographic Flexibility:** A resource group has a location (to store metadata), but the resources inside it can reside in **different regions**.
  - **Atomic Deletion:** Deleting a resource group (`az group delete`) permanently cascades and deletes every single resource contained inside it.
  - **Tagging & Cost Tracking:** Resource groups serve as the primary target for metadata tags (`Environment=Production`, `Owner=FinOps`).

---

## ⚖️ 3. Master Comparison Table

| Feature / Dimension | Management Group | Subscription | Resource Group | Resource |
| :--- | :--- | :--- | :--- | :--- |
| **Primary Purpose** | Enterprise governance at scale | Billing unit & quota boundary | Lifecycle & deployment container | The actual cloud asset |
| **Contains** | Other MGs or Subscriptions | Resource Groups | Individual Resources | N/A (Atomic object) |
| **Invoiced / Billed?** | ❌ No | ✅ **Yes (Monthly Invoice)** | ❌ No | Billed to parent Subscription |
| **Policy Scope?** | ✅ Yes (Inherited downward) | ✅ Yes (Inherited downward) | ✅ Yes (Inherited downward) | ✅ Yes (Directly assigned) |
| **RBAC Scope?** | ✅ Yes (Inherited downward) | ✅ Yes (Inherited downward) | ✅ Yes (Inherited downward) | ✅ Yes (Directly assigned) |
| **Maximum Limit** | 10,000 per tenant (6 levels) | Unlimited per tenant | 980 per subscription | Varies by resource type |

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← Prev Section (11 - Cloud Computing - AWS)](../../11%20-%20Cloud%20Computing%20-%20AWS/14-AWS-Cost-Optimization-FinOps-and-Migration-Strategies/12-Quick-Revision.md) | [Index](../../../README.md) | [02 - Resource Groups Architecture and Lifecycle →](./02-Resource-Groups-Architecture-and-Lifecycle.md) |
