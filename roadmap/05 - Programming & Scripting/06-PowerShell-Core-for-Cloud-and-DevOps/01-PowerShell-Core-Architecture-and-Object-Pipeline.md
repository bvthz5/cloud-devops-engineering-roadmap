# 01 - PowerShell Core Architecture and Object Pipeline

## 1. Architectural Foundations: Object Pipeline vs Byte Stream

Traditional shells (Bash, Zsh) communicate across processes using standard streams (`stdin`, `stdout`, `stderr`) as unformatted character or byte streams. The consuming process must parse, tokenize, and regex-match text using tools like `grep`, `awk`, `sed`, or `jq`.

PowerShell Core (`pwsh`) executes within the **.NET Core Common Language Runtime (CLR)**. Every command (Cmdlet, function, or script block) outputs live **.NET objects** (`System.Object` derivatives) with typed properties, methods, and metadata.

```text
Traditional Unix Pipeline (Raw Text Stream):
[ ps aux ] ---> (ASCII Text Stream) ---> [ grep nginx ] ---> [ awk '{print $2}' ] ---> [ kill -9 ]
                      ^                         ^
               (Fragile Parsing)         (Column Offset Bug)

PowerShell Object Pipeline (.NET Objects):
[ Get-Process ] ---> (System.Diagnostics.Process[]) ---> [ Where-Object Name -eq 'nginx' ] ---> [ Stop-Process ]
                             ^                                      ^
                      (Typed Object:                          (Direct Property
                     .Id, .CPU, .Path)                          Evaluation)
```

### Key Architectural Advantages
1. **Zero Text Parsing Overhead**: No string-splitting or regex extraction for downstream consumers.
2. **Type Safety & Rich Introspection**: Objects preserve type definitions (`DateTime`, `IPAddress`, `Process`, `PSCustomObject`).
3. **Pipelining Semantics**: Pipeline items are streamed one-by-one (lazy evaluation) rather than collected entirely into memory by default, maintaining low memory footprints for large collections.

---

## 2. Core Pipeline Cmdlets: Filtering, Selecting, and Iterating

### 2.1 Filtering with `Where-Object`
`Where-Object` (alias `?` or `where`) filters objects passing through the pipeline based on script blocks or simplified property comparisons.

```powershell
# Simplified syntax (Fast & Readable)
Get-Service | Where-Object Status -eq 'Running'

# ScriptBlock syntax (Complex evaluations)
Get-Process | Where-Object { $_.WorkingSet64 -gt 500MB -and $_.CPU -gt 30.0 }
```

### 2.2 Projection with `Select-Object`
`Select-Object` (alias `select`) limits the number of objects or projects specific object properties into a new synthetic `PSCustomObject`.

```powershell
# Select specific properties & calculated properties
Get-Process | Select-Object -Property Id, ProcessName, @{
    Name       = 'MemoryMB'
    Expression = { [math]::Round($_.WorkingSet64 / 1MB, 2) }
} -First 10
```

### 2.3 Iteration with `ForEach-Object`
`ForEach-Object` (alias `%` or `foreach`) executes a script block for each item streamed through the pipeline. In PowerShell 7+, it supports native multithreading via `-Parallel`.

```powershell
# Sequential processing
$servers = @('web01', 'web02', 'web03', 'db01')
$servers | ForEach-Object {
    Write-Host "Pinging $_..."
    Test-Connection -TargetName $_ -Count 1 -Quiet
}

# High-throughput parallel execution (PowerShell 7+)
$servers | ForEach-Object -Parallel {
    $server = $_
    $res = Test-Connection -TargetName $server -Count 2 -Quiet
    [PSCustomObject]@{
        Server = $server
        Online = $res
    }
} -ThrottleLimit 10
```

---

## 3. Abstract Syntax Tree (AST) & Execution Engine

PowerShell parses commands into an Abstract Syntax Tree (`System.Management.Automation.Language.Ast`) prior to compilation into .NET Intermediate Language (IL) via DLR (Dynamic Language Runtime). This architecture allows tools like `PSScriptAnalyzer` to parse and lint scripts without runtime execution.

```powershell
# Inspecting AST dynamically
$script = { Get-Process | Stop-Process -Force }
$ast = [System.Management.Automation.Language.Parser]::ParseInput($script, [ref]$null, [ref]$null)
$ast.FindAll({ $args[0] -is [System.Management.Automation.Language.CommandAst] }, $true) | Select-Object CommandElements
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← Prev Module (05-Automation-Script-Templates)](../05-Automation-Script-Templates/12-Quick-Revision.md) | [Index](../../../README.md) | [02 - Cross Platform PowerShell on Linux and Containers →](./02-Cross-Platform-PowerShell-on-Linux-and-Containers.md) |
