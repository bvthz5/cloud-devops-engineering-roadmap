# 03 - Advanced Functions, ScriptBlocks, and Modules

## 1. Advanced Functions (`[CmdletBinding()]`)

An advanced function behaves identically to a compiled C# binary Cmdlet. By decorating a function with `[CmdletBinding()]`, PowerShell injects standard enterprise parameters: `-Verbose`, `-Debug`, `-ErrorAction`, `-ErrorVariable`, `-WarningAction`, and support for `-WhatIf` / `-Confirm`.

```powershell
function Invoke-CloudStorageCleanup {
    [CmdletBinding(SupportsShouldProcess = $true, ConfirmImpact = 'High')]
    param(
        [Parameter(Mandatory = $true, ValueFromPipeline = $true, ValueFromPipelineByPropertyName = $true)]
        [ValidateNotNullOrEmpty()]
        [string]$BucketName,

        [Parameter(Mandatory = $false)]
        [ValidateRange(1, 365)]
        [int]$RetentionDays = 30,

        [Parameter(Mandatory = $false)]
        [switch]$DryRun
    )

    begin {
        Write-Verbose "Initializing Cloud Storage Cleanup job. Threshold: $RetentionDays days."
        $deletedCount = 0
    }

    process {
        Write-Verbose "Evaluating bucket: $BucketName"
        
        # Guard clause using ShouldProcess (-WhatIf / -Confirm support)
        if ($PSCmdlet.ShouldProcess("Bucket: $BucketName", "Purge objects older than $RetentionDays days")) {
            if ($DryRun) {
                Write-Information "DRY-RUN: Would inspect and purge objects from $BucketName"
            } else {
                Write-Verbose "Executing deletion API call against $BucketName..."
                # Invocation logic here
                $deletedCount++
            }
        }
    }

    end {
        Write-Verbose "Cleanup process completed. Total buckets targeted: $deletedCount."
        return [PSCustomObject]@{
            Status         = 'Success'
            BucketsTargeted = $deletedCount
            RetentionDays  = $RetentionDays
            Timestamp      = (Get-Date).ToUniversalTime().ToString('o')
        }
    }
}
```

### The Three Execution Phases
1. **`begin {}`**: Executes **once** before any pipeline items arrive. Used for establishing client connections, initializing counters, and validating global configuration.
2. **`process {}`**: Executes **once per item** passed through the pipeline. Mandatory when binding `ValueFromPipeline = $true`.
3. **`end {}`**: Executes **once** after all items have traversed the pipeline. Used for closing database sessions, flushing buffers, and emitting aggregate telemetry.

---

## 2. ScriptBlocks: Dynamic Code as First-Class Citizens

ScriptBlocks (`{ ... }`) are anonymous closures in PowerShell that can be stored in variables, passed as parameters, or dispatched across threads and remote systems.

```powershell
# Defining a ScriptBlock with parameters
$healthCheck = {
    param([string]$Url, [int]$TimeoutSeconds = 5)
    
    try {
        $response = Invoke-WebRequest -Uri $Url -TimeoutSec $TimeoutSeconds -UseBasicParsing -Method Head
        return [PSCustomObject]@{
            Url        = $Url
            StatusCode = $response.StatusCode
            LatencyMs  = $response.RawContentLength
            Healthy    = ($response.StatusCode -ge 200 -and $response.StatusCode -lt 400)
        }
    } catch {
        return [PSCustomObject]@{
            Url        = $Url
            StatusCode = 0
            LatencyMs  = -1
            Healthy    = $false
            Error      = $_.Exception.Message
        }
    }
}

# Invoking dynamically via call operator (&)
$result = & $healthCheck -Url "https://api.github.com"
```

---

## 3. Production Module Packaging (`.psm1` & `.psd1`)

A well-structured production PowerShell module contains:
- **Module Manifest (`MyModule.psd1`)**: Metadata, version, export restrictions, and required modules.
- **Script Module (`MyModule.psm1`)**: Implementation code and internal helper logic.

### 3.1 Module Manifest (`DevOpsToolkit.psd1`)
```powershell
@{
    RootModule           = 'DevOpsToolkit.psm1'
    ModuleVersion        = '1.2.0'
    GUID                 = '9e414c1f-9a1b-4d4b-9721-3e4b78912345'
    Author               = 'Platform Engineering Team'
    CompanyName          = 'Enterprise Cloud'
    PowerShellVersion    = '7.0'
    FunctionsToExport    = @('Invoke-CloudStorageCleanup', 'Get-KubeClusterHealth')
    CmdletsToExport      = @()
    VariablesToExport    = @()
    AliasesToExport      = @('cleanup-bucket')
}
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [02 - Cross-Platform PowerShell](./02-Cross-Platform-PowerShell-on-Linux-and-Containers.md) | [README](./README.md) | [04 - Error Handling and Streams](./04-Error-Handling-Streams-and-Idempotency.md) |
