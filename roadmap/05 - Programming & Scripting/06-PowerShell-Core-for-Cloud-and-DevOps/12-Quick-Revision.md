# 12 - Quick-Revision & Enterprise Cheat Sheet

```powershell
# Strict Mode & Error Handling Best Practice
Set-StrictMode -Version Latest
$ErrorActionPreference = 'Stop'

# Pipeline Filtering & Projection
Get-Service | Where-Object Status -eq 'Running' | Select-Object -Property Name, DisplayName

# PowerShell 7 Parallel Loop
$items | ForEach-Object -Parallel {
    $item = $_
    Write-Host "Worker processing $item from $($using:batchId)"
} -ThrottleLimit 8

# Null-Coalescing & Ternary
$env = $targetEnv ?? 'Development'
$status = $isProd ? 'CRITICAL' : 'NORMAL'

# Native Exit Code Guard
& docker build -t myimage .
if ($LASTEXITCODE -ne 0) { throw "Docker build failed with code $LASTEXITCODE" }
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [11 - Multiple-Choice Assessment](./11-MCQ.md) | [README](./README.md) | [07 - Cloud SDKs & Infrastructure Automation](../07-Cloud-SDKs-and-Infrastructure-Automation/README.md) |
