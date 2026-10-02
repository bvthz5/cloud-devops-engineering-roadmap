# 08 - Troubleshooting & Diagnostic Trees

## 1. PowerShell Diagnostic Tree

```text
Problem: Automation Script Fails or Behaves Erratically
  │
  ├──► Did the script fail silently?
  │      ├── Yes: Check $ErrorActionPreference. Is it 'Continue'? Set to 'Stop'.
  │      └── Check $LASTEXITCODE vs $?:
  │            - Native Linux/Windows binary errors only update $LASTEXITCODE!
  │            - Test: if ($LASTEXITCODE -ne 0) { throw "Native command failed" }
  │
  ├──► Is the issue related to parameter parsing / pipeline binding?
  │      ├── Run: Trace-Command -Name ParameterBinding -Expression { My-Cmdlet -Param val } -PSHost
  │      └── Inspect AST: [System.Management.Automation.Language.Parser]::ParseInput(...)
  │
  └──► Is execution timing out or hanging?
         ├── Run: Set-PSDebug -Trace 2 (Prints each statement before execution)
         └── Inspect open network sockets or deadlocks in -Parallel threads.
```

---

## 2. `$LASTEXITCODE` vs `$?` Gotchas

A very common trap when executing native CLI commands (e.g., `git`, `docker`, `kubectl`, `terraform`) from PowerShell:

```powershell
# Calling a native CLI command
git checkout non-existent-branch 2>$null

# $? evaluates whether the LAST POWERSHELL COMMAND succeeded
# $LASTEXITCODE holds the process exit code of the native binary (0 = success, non-zero = fail)

Write-Host "`$? is: $?"                   # Might still be True!
Write-Host "`$LASTEXITCODE is: $LASTEXITCODE" # Correctly reflects 1 or 128

# Enterprise safe execution pattern:
function Invoke-NativeCommand {
    param([scriptblock]$Command)
    
    & $Command
    if ($LASTEXITCODE -ne 0) {
        throw "Native binary execution failed with exit code $LASTEXITCODE"
    }
}
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [07 - Real-World Scenarios](./07-Real-World-Scenarios.md) | [README](./README.md) | [09 - Interview Questions](./09-Interview-QA.md) |
