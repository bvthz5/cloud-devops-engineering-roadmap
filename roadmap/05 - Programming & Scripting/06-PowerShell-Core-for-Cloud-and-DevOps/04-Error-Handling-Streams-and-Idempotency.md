# 04 - Error Handling, Streams, and Idempotency

## 1. Terminating vs Non-Terminating Errors

PowerShell differentiates between two fundamentally distinct error classes:

1. **Non-Terminating Errors (Default)**: Emitted by cmdlets when an individual operation fails (e.g., file not found in a list of 100 files). The script logs the error to the Error stream and **continues executing** the next line or loop iteration!
2. **Terminating Errors**: Caused by catastrophic failures (syntax errors, missing script files, or explicit `throw` statements). These immediately halt execution unless trapped by a `try/catch` block.

```powershell
# DANGEROUS DEFAULT BEHAVIOR:
# If Remove-Item fails, Write-Host STILL RUNS!
Remove-Item -Path "/non/existent/path"
Write-Host "Script continued despite failure!" -ForegroundColor Red

# ENTERPRISE BEST PRACTICE: Force all errors to be terminating
$ErrorActionPreference = 'Stop'

try {
    Remove-Item -Path "/non/existent/path"
    Write-Host "This will NOT be reached."
} catch [System.IO.ItemNotFoundException] {
    Write-Warning "Caught specific missing item: $($_.Exception.Message)"
} catch {
    Write-Error "Caught unhandled fatal exception: $($_.Exception.Message)"
    exit 1
} finally {
    Write-Verbose "Cleanup and audit logging completed."
}
```

---

## 2. The 6 Output Streams

PowerShell isolates output into distinct numbered streams, preventing diagnostic text from contaminating the data pipeline.

```text
Stream 1: Success / Output Stream (Objects sent to pipeline: Write-Output, return)
Stream 2: Error Stream            (Exceptions & error objects: Write-Error)
Stream 3: Warning Stream          (Operational warnings: Write-Warning)
Stream 4: Verbose Stream          (Detailed diagnostic tracing: Write-Verbose)
Stream 5: Debug Stream            (Interactive debugger information: Write-Debug)
Stream 6: Information Stream      (Structured host messages & telemetry: Write-Information)
```

```powershell
function Test-Streams {
    [CmdletBinding()]
    param()
    
    Write-Output "Pipeline Object (Stream 1)"         # Capturable in variable: $res = Test-Streams
    Write-Error "Error Message (Stream 2)"
    Write-Warning "Warning Warning (Stream 3)"
    Write-Verbose "Verbose details enabled with -Verbose (Stream 4)"
    Write-Debug "Debug trace enabled with -Debug (Stream 5)"
    Write-Information "Informational message (Stream 6)"
}
```

### Stream Redirection Operators
```powershell
# Redirect Error (2) to Output (1)
./run-task.ps1 2>&1 | Tee-Object -FilePath "task.log"

# Suppress Warnings (3) and Information (6)
./run-task.ps1 3>$null 6>$null
```

---

## 3. Idempotent Automation Design with `ShouldProcess`

In production pipelines, scripts must be **idempotent**: executing a script five times must produce the exact same system state as executing it once, without duplicating resources or throwing false errors.

```powershell
function Set-CloudServiceConfiguration {
    [CmdletBinding(SupportsShouldProcess = $true)]
    param(
        [Parameter(Mandatory = $true)]
        [string]$ConfigKey,

        [Parameter(Mandatory = $true)]
        [string]$DesiredValue
    )

    $currentValue = (Get-ItemProperty -Path "HKLM:\Software\MyApp" -Name $ConfigKey -ErrorAction SilentlyContinue).$ConfigKey

    if ($currentValue -eq $DesiredValue) {
        Write-Verbose "Key '$ConfigKey' is already at desired value '$DesiredValue'. No action needed."
        return
    }

    if ($PSCmdlet.ShouldProcess("Registry Key '$ConfigKey'", "Update value from '$currentValue' to '$DesiredValue'")) {
        Set-ItemProperty -Path "HKLM:\Software\MyApp" -Name $ConfigKey -Value $DesiredValue -Force
        Write-Information "Successfully updated '$ConfigKey' to '$DesiredValue'."
    }
}
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 03 - Advanced Functions Script Blocks and Modules](./03-Advanced-Functions-Script-Blocks-and-Modules.md) | [Index](../../../README.md) | [05 - Azure PowerShell Az Module and Automation →](./05-Azure-PowerShell-Az-Module-and-Automation.md) |
