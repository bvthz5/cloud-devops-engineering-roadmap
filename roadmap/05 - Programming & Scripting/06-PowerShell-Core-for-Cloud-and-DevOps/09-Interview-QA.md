# 09 - Interview Questions & Architectural Scenarios

### Q1: How does the PowerShell pipeline fundamentally differ from Unix pipelines like Bash?
**Answer**: Unix pipelines stream raw, untyped byte/ASCII streams between processes. The receiving program must parse text using regular expressions or column-delimiters. PowerShell pipelines stream live **.NET objects**. Downstream cmdlets directly access typed properties (e.g., `$_.CPU`, `$_.Size.TotalGigaBytes`) without string manipulation.

### Q2: What is the difference between `$ErrorActionPreference = 'Stop'` and using `try/catch`?
**Answer**: In PowerShell, standard cmdlets generate non-terminating errors by default when encountering failures. `try/catch` blocks **only catch terminating errors**. Without `$ErrorActionPreference = 'Stop'` (or `-ErrorAction Stop` on the cmdlet), a non-terminating error bypasses the `catch` block and execution continues.

### Q3: How does `ForEach-Object -Parallel` work in PowerShell 7, and how do variables cross runspace boundaries?
**Answer**: `ForEach-Object -Parallel` spins up a pool of separate .NET runspaces (threads). Because each runspace has isolated scope, variables defined outside the block cannot be read directly unless referenced via the `$using:` scope modifier (e.g., `$using:targetServer`).

### Q4: Explain the purpose of `SupportsShouldProcess` in `[CmdletBinding()]`.
**Answer**: It enables native support for `-WhatIf` and `-Confirm` parameters. When implemented with `$PSCmdlet.ShouldProcess("Target", "Action")`, it allows engineers to test destructive scripts safely in dry-run mode before executing real changes.

### Q5: How do you handle secrets and secure strings across cross-platform scripts?
**Answer**: Avoid storing plaintext secrets. In PowerShell 7, use `SecretManagement` and `SecretStore` modules to interface with local or enterprise secret vaults (Azure Key Vault, HashiCorp Vault, AWS Secrets Manager).

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [08 - Troubleshooting](./08-Troubleshooting.md) | [README](./README.md) | [10 - Hands-On Practice](./10-Hands-On-Practice.md) |
