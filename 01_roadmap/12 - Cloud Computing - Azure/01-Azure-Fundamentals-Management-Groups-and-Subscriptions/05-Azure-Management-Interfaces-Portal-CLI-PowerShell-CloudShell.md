# 05 - Azure Management Interfaces: Portal, CLI, PowerShell & Cloud Shell

> **Focus:** Interacting with Azure Resource Manager (ARM), Azure Portal, Azure CLI (`az`), Azure PowerShell (`Az` module), and browser-based Azure Cloud Shell.

---

## 🖥️ 1. The 4 Azure Management Tools

```text
┌────────────────────────────────────────────────────────┐
│               AZURE MANAGEMENT INTERFACES              │
│                                                        │
│  1. Azure Portal ──────► Web GUI at portal.azure.com   │
│  2. Azure CLI (`az`) ──► Cross-platform bash/cmd tool  │
│  3. Azure PowerShell ──► PowerShell `Az` cmdlets        │
│  4. Azure Cloud Shell ─► Browser-based shell with auth │
└───────────────────────────┬────────────────────────────┘
                            │ All call the exact same REST API
                            ▼
┌────────────────────────────────────────────────────────┐
│            Azure Resource Manager (ARM) Engine         │
└────────────────────────────────────────────────────────┘
```

---

## 💻 2. Tool Comparison & When to Use What

| Tool | Environment | Best Use Case | Automation Ready? |
| :--- | :--- | :--- | :---: |
| **Azure Portal** | Web Browser | Exploring services, visual monitoring dashboards, one-off admin tasks. | ❌ No |
| **Azure CLI (`az`)** | Linux, macOS, Windows | Linux engineers, CI/CD pipelines (GitHub Actions), Docker scripts. | ✅ **Yes** |
| **Azure PowerShell** | Windows, macOS, Linux | Windows sysadmins, .NET enterprise shops, automated pipeline tasks. | ✅ **Yes** |
| **Azure Cloud Shell** | Built into Portal | Pre-authenticated browser terminal; pre-installed with Git, Terraform, az CLI. | ✅ **Yes** |

---

### 💻 Master Azure CLI & PowerShell Command Recipes

```bash
# 1. Login to Azure CLI
az login

# 2. Set default active subscription
az account set --subscription "Production-Subscription"

# 3. Create Resource Group
az group create --name rg-workloads --location eastus

# 4. List all VMs in formatted table
az vm list --output table
```

```powershell
# PowerShell Az Module Syntax
Connect-AzAccount
Set-AzContext -Subscription "Production-Subscription"
New-AzResourceGroup -Name "rg-workloads" -Location "EastUS"
Get-AzVM | Format-Table Name, ResourceGroupName, Location
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 04 - Azure Global Infrastructure Regions Availability Zones Geos](./04-Azure-Global-Infrastructure-Regions-Availability-Zones-Geos.md) | [Index](../../../README.md) | [06 - Azure Resource Manager ARM Engine Architecture →](./06-Azure-Resource-Manager-ARM-Engine-Architecture.md) |
