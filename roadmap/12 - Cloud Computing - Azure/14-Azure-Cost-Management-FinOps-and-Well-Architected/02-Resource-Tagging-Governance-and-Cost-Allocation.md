# 02 - Resource Tagging Governance: Schema Taxonomy & Cost Allocation Rules

> **Focus:** Metadata governance and financial attribution in Microsoft Azure. Deep dive into standardized Enterprise Tagging taxonomies, Azure Policy tag enforcement (`Modify` and `Deny` effects), Cost Allocation Rules for shared infrastructure, and automating tag propagation.

---

## 🏷️ 1. What, Why, and History of Resource Tagging

### What are Azure Resource Tags?
Resource Tags are key-value metadata pairs (e.g., `Environment: Production`, `CostCenter: CC-4100`) attached directly to Azure resources, resource groups, or subscriptions.

### Why is an Enterprise Tagging Strategy Mandatory?
1. **Financial Showback & Chargeback:** Without tags, finance cannot determine which portion of a $50,000 monthly cloud invoice belongs to Marketing, HR, or Core Engineering.
2. **Operations Automation:** Scheduled automation runbooks inspect tags (e.g., `AutoShutdown: Mon-Fri-19:00`) to power off dev VMs at night, cutting non-production compute bills by 65%.
3. **Security Context & Incident Response:** When a security breach alert fires on a VM, the incident response team immediately checks tags (`Owner`, `Criticality`, `DataClassification`) to assess business impact.

---

## 📋 2. Standard Enterprise Tagging Schema Taxonomy

An enterprise cloud governance framework enforces five standardized mandatory tags across 100% of resources:

| Tag Key | Allowed Values / Format | Operational Purpose |
| :--- | :--- | :--- |
| **`Environment`** | `prod`, `staging`, `dev`, `qa`, `sandbox` | Distinguishes production workloads from throwaway test labs |
| **`CostCenter`** | `CC-1010`, `CC-2040`, `CC-9900` | Financial accounting code for ERP and invoice chargeback |
| **`Owner`** | Corporate email address (`lead@contoso.com`) | Designates the primary engineering contact for alerts/changes |
| **`Project`** | Standardized project code (`Mobile-App-v2`) | Aggregates all resources supporting a specific business product |
| **`Criticality`** | `Tier-1-MissionCritical`, `Tier-2`, `Tier-3` | Dictates backup frequency, DR SLA, and on-call response speed |

---

## 🔄 3. Azure Cost Allocation Rules: Splitting Shared Infrastructure

A major challenge in cloud economics is **Shared Infrastructure Costs** (e.g., a shared AKS cluster, a central hub Virtual Network, or an ExpressRoute circuit utilized by 10 departments):

```text
┌────────────────────────────────────────────────────────────────────────────────────────┐
│                              AZURE COST ALLOCATION RULES                               │
├────────────────────────────────────────────────────────────────────────────────────────┤
│ SHARED PARENT RESOURCE:                                                                │
│   • Resource Group: rg-hub-networking (ExpressRoute & Azure Firewall)                   │
│   • Total Monthly Spend: $6,000 / month                                                │
│                                                                                        │
│ COST ALLOCATION RULE DEFINITION:                                                       │
│   • Automatically redistributes costs from the parent to target subscriptions:         │
│     ├── 50% ($3,000) allocated to: Subscription "Retail-Banking-Prod"                  │
│     ├── 30% ($1,800) allocated to: Subscription "Wealth-Management-Prod"               │
│     └── 20% ($1,200) allocated to: Subscription "Insurance-Prod"                       │
│                                                                                        │
│ RESULT IN COST REPORTS:                                                                │
│   • Shared networking spend is transparently attributed to consumer business units!    │
└────────────────────────────────────────────────────────────────────────────────────────┘
```

---

## 🛡️ 4. Automating Tag Propagation via Azure Policy (`Modify` Effect)

Rather than forcing developers to manually tag 50 resources in a template, use an Azure Policy that **automatically inherits tags from the parent Resource Group down to all child resources**:

```json
{
  "properties": {
    "displayName": "Inherit CostCenter tag from Resource Group",
    "policyRule": {
      "if": {
        "allOf": [
          { "field": "tags['CostCenter']", "exists": "false" },
          { "field": "tags['CostCenter']", "notEquals": "", "source": "action", "resourceGroupName": "[resourceGroup().name]" }
        ]
      },
      "then": {
        "effect": "modify",
        "details": {
          "roleDefinitionIds": [
            "/providers/microsoft.authorization/roleDefinitions/b24988ac-6180-42a0-ab88-20f7382dd24c"
          ],
          "operations": [
            {
              "operation": "add",
              "field": "tags['CostCenter']",
              "value": "[resourceGroup().tags['CostCenter']]"
            }
          ]
        }
      }
    }
  }
}
```

---

## 💻 5. Practical CLI: Applying Tags at Scale

```bash
# 1. Apply tags to an entire Resource Group
az group update \
  --name "rg-analytics-prod" \
  --set tags.Environment="prod" tags.CostCenter="CC-4400" tags.Owner="data-team@contoso.com"

# 2. Query all resources missing the 'CostCenter' tag via Azure Resource Graph
az graph query -q "resources | where isnull(tags.CostCenter) | project name, type, resourceGroup, location"
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 01 - Azure Cost Management and Billing Architecture](./01-Azure-Cost-Management-and-Billing-Architecture.md) | [Index](../../../README.md) | [03 - Azure Reservations and Savings Plans Cost Optimization →](./03-Azure-Reservations-and-Savings-Plans-Cost-Optimization.md) |
