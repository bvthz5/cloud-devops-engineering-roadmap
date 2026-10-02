# 06 - PowerShell Core for Cloud and DevOps

PowerShell Core (`pwsh`) is a cross-platform (Windows, Linux, macOS) task automation and configuration management framework built on modern .NET. Unlike traditional Unix shells that pipe raw ASCII/UTF-8 byte streams, PowerShell operates on an **object pipeline**, passing rich, structured .NET objects between commands. This architectural foundation makes it an indispensable tool for cloud engineering, cross-platform cloud SDKs (Azure `Az`, AWS Tools for PowerShell), and enterprise platform automation.

---

## Module Index

| # | Topic | Description |
|---|---|---|
| 01 | [PowerShell Core Architecture & Object Pipeline](./01-PowerShell-Core-Architecture-and-Object-Pipeline.md) | .NET Core CLR runtime, structured object pipeline, AST, streaming semantics. |
| 02 | [Cross-Platform PowerShell on Linux & Containers](./02-Cross-Platform-PowerShell-on-Linux-and-Containers.md) | Running pwsh on Linux, containerized pipelines, POSIX interop, `/` vs `\` handling. |
| 03 | [Advanced Functions, ScriptBlocks & Modules](./03-Advanced-Functions-Script-Blocks-and-Modules.md) | `[CmdletBinding()]`, pipeline binding, `Begin/Process/End`, custom `.psm1`/`.psd1` packaging. |
| 04 | [Error Handling, Streams & Idempotency](./04-Error-Handling-Streams-and-Idempotency.md) | Terminating vs non-terminating errors, 6 output streams, `try/catch`, `-WhatIf`/`-Confirm`. |
| 05 | [Azure PowerShell (Az Module) Automation](./05-Azure-PowerShell-Az-Module-and-Automation.md) | Service Principals, Managed Identities, Azure Runbooks, high-throughput batch operations. |
| 06 | [AWS & Multi-Cloud Tools for PowerShell](./06-AWS-and-Multi-Cloud-Tools-for-PowerShell.md) | `AWS.Tools` modular architecture, IAM roles, S3/EC2 automation, hybrid directory integration. |
| 07 | [Real-World Scenarios](./07-Real-World-Scenarios.md) | Enterprise post-mortems: Runbook permission disaster, pipeline memory exhaustion, path bugs. |
| 08 | [Troubleshooting & Diagnostic Trees](./08-Troubleshooting.md) | `Set-PSDebug`, AST inspection, `$LASTEXITCODE` vs `$?`, SSH/WinRM remoting diagnostics. |
| 09 | [Interview Questions & Architectural Scenarios](./09-Interview-QA.md) | 10 senior DevOps/SRE questions covering pipeline binding, concurrency, and security. |
| 10 | [Hands-On Practice Labs](./10-Hands-On-Practice.md) | Lab 1: Linux pwsh container; Lab 2: Az Managed Identity script; Lab 3: Idempotent tool. |
| 11 | [Multiple-Choice Assessment (MCQ)](./11-MCQ.md) | 10 deep technical scenario MCQs with rationale. |
| 12 | [Quick-Revision & Enterprise Cheat Sheet](./12-Quick-Revision.md) | High-density reference for syntax, cmdlets, error preferences, and common pipeline idioms. |

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [05 - Automation Script Templates](../05-Automation-Script-Templates/README.md) | [README](./README.md) | [01 - PowerShell Core Architecture](./01-PowerShell-Core-Architecture-and-Object-Pipeline.md) |
