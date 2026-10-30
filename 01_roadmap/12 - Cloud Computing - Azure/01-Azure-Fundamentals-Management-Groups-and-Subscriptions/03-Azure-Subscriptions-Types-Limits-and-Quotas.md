# 03 - Azure Subscriptions: Types, Limits, Quotas & Multi-Subscription Design

> **Focus:** Azure billing boundaries, subscription types (Pay-As-You-Go, Enterprise Agreement, CSP, Dev/Test), regional resource quotas, and multi-subscription isolation patterns.

---

## 💳 1. What a Subscription Does

A **Subscription** is the fundamental billing and resource boundary in Azure. Every resource (VM, database, VNet) lives inside exactly one subscription:
- **Billing Boundary:** Each subscription generates its own independent invoice and cost breakdown.
- **Trust Boundary:** Each subscription trusts exactly one Microsoft Entra ID (AAD) tenant.
- **Resource Quota Container:** Enforces hard and soft quotas per region (e.g., maximum 25,000 VMs or 20,000 vCores per region).

---

## 📊 2. Subscription Types Comparison

| Subscription Type | Target Audience | Key Characteristics |
| :--- | :--- | :--- |
| **Pay-As-You-Go** | Individuals & Small Startups | Monthly credit card billing with zero upfront commitment. |
| **Enterprise Agreement (EA)** | Large Enterprises | Multi-year monetary commitment with enterprise volume discounts. |
| **Microsoft Customer Agreement (MCA)**| Modern Corporate standard | Replaces traditional EA with unified digital portal billing. |
| **Azure Dev/Test** | Development teams | Heavily discounted software licensing rates (Windows Server at Linux rates). |
| **Cloud Solution Provider (CSP)** | Partner-managed clients | Billed and managed directly through a Microsoft certified partner. |
| **Free Trial** | New evaluators | $200 free credit for 30 days + 12 months of popular free services. |

---

## 🏢 3. Enterprise Multi-Subscription Pattern

```text
Contoso Corporation (Entra ID Tenant: contoso.onmicrosoft.com)
├── Subscription: Production-Workloads       ← Live customer revenue apps
├── Subscription: Staging-PreProd            ← Exact mirror of production
├── Subscription: Development-Sandboxes      ← Dev/Test discounted rates
└── Subscription: Core-Shared-Services       ← Centralized ExpressRoute & Hub VNet
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 02 - Resource Groups Architecture and Lifecycle](./02-Resource-Groups-Architecture-and-Lifecycle.md) | [Index](../../../README.md) | [04 - Azure Global Infrastructure Regions Availability Zones Geos →](./04-Azure-Global-Infrastructure-Regions-Availability-Zones-Geos.md) |
