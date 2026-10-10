# 03 - Azure Policy Architecture: Definitions, Initiatives & Policy-as-Code

> **Focus:** Automated platform governance and compliance in Azure. Deep dive into Azure Policy vs. Azure RBAC, the 7 Policy Effects (`Deny`, `Audit`, `Append`, `Modify`, `DeployIfNotExists`), Initiatives (Policy Sets), Evaluation Cycles, and automated remediation tasks.

---

## 🏛️ 1. What, Why, and History of Azure Policy

### What is Azure Policy?
Azure Policy is a governance engine built directly into the Azure Resource Manager (ARM) control plane that evaluates Azure resource configurations against business rules, security benchmarks, and regulatory compliance standards.

### Why is RBAC Not Enough for Cloud Governance?
A fundamental architectural principle in Azure is the distinction between **RBAC** and **Policy**:
- **Azure RBAC answers: "WHO can do WHAT?"** (e.g., Can Sarah create a Virtual Machine?).
- **Azure Policy answers: "WHAT CAN IT LOOK LIKE?"** (e.g., Even if Sarah is an Owner, she cannot create a VM that lacks disk encryption, lacks an Environment tag, or is deployed in an unauthorized region).

Without Azure Policy, a developer with Contributor rights could accidentally spin up a $25,000/month GPU cluster or deploy an unencrypted storage account exposed to the public internet.

### 📜 History & Evolution:
- **2017:** Azure Policy launches to replace legacy static JSON tags and manual audits.
- **2019:** Introduction of **DeployIfNotExists (DINE)** and **Modify** effects with automated remediation tasks.
- **2021–Present:** Full integration into Azure Landing Zones (Enterprise-Scale) and GitHub/Azure DevOps CI/CD pipelines via Policy-as-Code.

---

## ⚖️ 2. The 7 Azure Policy Effects Explained

When an ARM request (create, update, or periodic scan) is evaluated, Azure Policy applies one of seven distinct effects:

```text
┌────────────────────────────────────────────────────────────────────────────────────────┐
│                              AZURE POLICY EVALUATION ORDER                             │
│                                                                                        │
│   ARM Request Arrives (az deployment / Portal / Terraform)                             │
│                           │                                                            │
│                           ▼                                                            │
│   1. Disabled: Policy rule is ignored.                                                 │
│   2. Append: Injects missing properties into request payload (e.g., adds tags).        │
│   3. Modify: Modifies tags or resource properties before ARM validation.               │
│   4. Deny: REJECTS THE REQUEST IMMEDIATELY (Fails deployment with HTTP 403 Forbidden). │
│   5. Audit: Allows deployment, but logs a Non-Compliant event in compliance dashboard. │
│   6. AuditIfNotExists: Audits if a dependent extension or log setting is missing.      │
│   7. DeployIfNotExists (DINE): Deploys missing resource via Managed Identity!          │
└────────────────────────────────────────────────────────────────────────────────────────┘
```

| Policy Effect | Action Taken During ARM Request | Use Case / Example |
| :--- | :--- | :--- |
| **`Deny`** | **Blocks deployment immediately** | Prevent unencrypted storage accounts or disallowed VM sizes |
| **`Audit`** | Allows deployment; marks non-compliant | Flag VMs missing backup without breaking developer pipelines |
| **`Append`** | Appends required fields to payload | Automatically append default tags if not specified |
| **`Modify`** | Adds, edits, or deletes tags/properties | Enforce standardized uppercase environment tags |
| **`DeployIfNotExists`**| **Deploys missing companion resources** | Automatically install Azure Monitor Agent (AMA) on every new VM |
| **`AuditIfNotExists`** | Audits if companion resource is missing | Audit whether Storage Accounts have Diagnostic Settings enabled |
| **`Disabled`** | Turns off rule evaluation | Testing or temporary emergency override |

---

## 📚 3. Definitions vs. Initiatives (Policy Sets)

```text
┌────────────────────────────────────────────────────────────────────────────────────────┐
│                              POLICY INITIATIVE ARCHITECTURE                            │
├────────────────────────────────────────────────────────────────────────────────────────┤
│ INITIATIVE (Policy Set): "Corporate Regulatory Security Baseline"                      │
│   ├── Policy Definition 1: Allowed locations (strictly 'eastus', 'westeurope')        │
│   ├── Policy Definition 2: Deny public blob access on storage accounts                 │
│   ├── Policy Definition 3: Require TLS 1.2 minimum on all PaaS services                │
│   ├── Policy Definition 4: Mandate Azure Backup on production virtual machines         │
│   └── Policy Definition 5: DeployIfNotExists Azure Monitor Agent on all VMs           │
│                               │                                                        │
│                               ▼ ASSIGNED AT MANAGEMENT GROUP SCOPE                     │
│   Inherited down to ALL 50 Subscriptions and thousands of Resource Groups!             │
└────────────────────────────────────────────────────────────────────────────────────────┘
```

---

## 💻 4. Practical Implementation: Defining and Assigning a Policy

### Policy Rule Definition (Restricting Allowed Regions to East US):
```json
{
  "properties": {
    "displayName": "Allowed Locations Policy",
    "description": "Restricts resource deployment to authorized corporate regions.",
    "mode": "Indexed",
    "parameters": {
      "allowedLocations": {
        "type": "Array",
        "defaultValue": [ "eastus", "westeurope" ]
      }
    },
    "policyRule": {
      "if": {
        "not": {
          "field": "location",
          "in": "[parameters('allowedLocations')]"
        }
      },
      "then": {
        "effect": "deny"
      }
    }
  }
}
```

### Assigning Policy via Azure CLI:
```bash
# 1. Create Policy Definition in Subscription
az policy definition create \
  --name "policy-restrict-regions" \
  --display-name "Restrict Allowed Azure Regions" \
  --rules policy-rule.json \
  --mode Indexed

# 2. Assign Policy at Subscription Scope
az policy assignment create \
  --name "assign-restrict-regions" \
  --policy "policy-restrict-regions" \
  --scope "/subscriptions/$(az account show --query id -o tsv)" \
  --params '{"allowedLocations": {"value": ["eastus", "eastus2"]}}'
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 02 - Key Vault Access Policies vs Azure RBAC Control](./02-Key-Vault-Access-Policies-vs-Azure-RBAC-Control.md) | [Index](../../../README.md) | [04 - Microsoft Defender for Cloud and Secure Score →](./04-Microsoft-Defender-for-Cloud-and-Secure-Score.md) |
