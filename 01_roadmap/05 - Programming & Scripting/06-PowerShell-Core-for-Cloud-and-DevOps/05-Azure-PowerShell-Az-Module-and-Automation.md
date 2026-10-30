# 05 - Azure PowerShell (Az Module) and Automation

## 1. Az Module Architecture and Authentication

The `Az` module is the official, open-source PowerShell module for managing Azure resources directly from Windows, Linux, and macOS. It replaces the legacy `AzureRM` module with optimized REST calls and .NET Core threading.

```text
[ PowerShell Core (pwsh) ]
           │
           ▼
[ Az.Accounts / Az.Compute / Az.Resources ]
           │
           ▼ (HTTPS / REST API)
[ Azure Resource Manager (ARM) / Microsoft Graph ]
```

### 1.1 Non-Interactive CI/CD Authentication Methods

#### Option A: Managed Identity (Inside Azure VMs, AKS, or Azure DevOps Agents)
```powershell
# Authenticate using system-assigned managed identity (Zero Credentials Stored)
Connect-AzAccount -Identity | Out-Null
Set-AzContext -SubscriptionId "00000000-0000-0000-0000-000000000000"
```

#### Option B: Service Principal with Client Secret / Certificate
```powershell
$tenantId     = $env:ARM_TENANT_ID
$clientId     = $env:ARM_CLIENT_ID
$clientSecret = $env:ARM_CLIENT_SECRET | ConvertTo-SecureString -AsPlainText -Force

$credential = New-Object System.Management.Automation.PSCredential($clientId, $clientSecret)
Connect-AzAccount -ServicePrincipal -Credential $credential -Tenant $tenantId | Out-Null
```

---

## 2. Enterprise Cloud Automation Scripts

### 2.1 Auditing and Purging Stale Untagged Resource Groups
```powershell
[CmdletBinding()]
param(
    [int]$GracePeriodDays = 7,
    [switch]$WhatIf
)

$threshold = (Get-Date).AddDays(-$GracePeriodDays)
$resourceGroups = Get-AzResourceGroup

foreach ($rg in $resourceGroups) {
    # Check if 'Environment' tag exists
    if (-not $rg.Tags -or -not $rg.Tags.ContainsKey('Environment')) {
        Write-Warning "Resource group $($rg.ResourceGroupName) is missing required 'Environment' tag!"
        
        # Check creation date or deployment history
        $lastDeployment = Get-AzResourceGroupDeployment -ResourceGroupName $rg.ResourceGroupName -ErrorAction SilentlyContinue |
                          Sort-Object Timestamp -Descending | Select-Object -First 1

        if ($lastDeployment -and $lastDeployment.Timestamp -lt $threshold) {
            Write-Host "Flagged for deletion: $($rg.ResourceGroupName) (Last deployment: $($lastDeployment.Timestamp))" -ForegroundColor Red
            
            if (-not $WhatIf) {
                Remove-AzResourceGroup -Name $rg.ResourceGroupName -Force -AsJob
            }
        }
    }
}
```

---

## 3. High-Throughput Batch Automation with `ForEach-Object -Parallel`

```powershell
# Scale out VM status collection across 50 subscriptions simultaneously
$subscriptions = Get-AzSubscription

$results = $subscriptions | ForEach-Object -Parallel {
    $sub = $_
    # Establish thread-safe Azure context inside parallel runspace
    Set-AzContext -SubscriptionId $sub.Id | Out-Null
    
    $vms = Get-AzVM -Status
    foreach ($vm in $vms) {
        [PSCustomObject]@{
            Subscription = $sub.Name
            VMName       = $vm.Name
            PowerState   = ($vm.Statuses | Where-Object Code -like 'PowerState/*').DisplayStatus
            Location     = $vm.Location
        }
    }
} -ThrottleLimit 10

$results | Export-Csv -Path "./azure-vm-inventory.csv" -NoTypeInformation
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 04 - Error Handling Streams and Idempotency](./04-Error-Handling-Streams-and-Idempotency.md) | [Index](../../../README.md) | [06 - AWS and Multi Cloud Tools for PowerShell →](./06-AWS-and-Multi-Cloud-Tools-for-PowerShell.md) |
