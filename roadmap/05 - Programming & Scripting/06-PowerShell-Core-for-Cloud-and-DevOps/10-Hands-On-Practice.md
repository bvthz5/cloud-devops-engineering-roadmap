# 10 - Hands-On Practice Labs

## Lab 1: Multi-Cloud Resource Tag Auditor

### Objective
Write an advanced PowerShell script with `[CmdletBinding()]` that inspects a list of cloud resources and generates a compliance JSON report identifying untagged assets.

### Implementation
```powershell
function Test-ResourceTagCompliance {
    [CmdletBinding()]
    param(
        [Parameter(Mandatory = $true, ValueFromPipeline = $true)]
        [PSCustomObject[]]$Resources,

        [string[]]$RequiredTags = @('Owner', 'Environment', 'CostCenter')
    )

    process {
        foreach ($item in $Resources) {
            $missing = @()
            foreach ($t in $RequiredTags) {
                if (-not $item.Tags.ContainsKey($t)) {
                    $missing += $t
                }
            }

            [PSCustomObject]@{
                ResourceId   = $item.Id
                Compliant    = ($missing.Count -eq 0)
                MissingTags  = $missing -join ','
                AuditDateUtc = (Get-Date).ToUniversalTime().ToString('s')
            }
        }
    }
}
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [09 - Interview Questions](./09-Interview-QA.md) | [README](./README.md) | [11 - Multiple-Choice Assessment](./11-MCQ.md) |
