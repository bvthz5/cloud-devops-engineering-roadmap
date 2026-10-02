# 02 - Cross-Platform PowerShell on Linux and Containers

## 1. PowerShell Core (`pwsh`) vs Windows PowerShell 5.1

Windows PowerShell 5.1 is tightly coupled to the proprietary Windows .NET Framework (running on `System32\WindowsPowerShell\v1.0\powershell.exe`). PowerShell Core 7.x (`pwsh`) is fully open-source and built on cross-platform .NET 6/8/9, running natively on Ubuntu, RHEL, Debian, Alpine Linux, macOS, and Windows.

| Feature | Windows PowerShell 5.1 | PowerShell 7.x (`pwsh`) |
|---|---|---|
| **Runtime Engine** | .NET Framework 4.5+ (Windows Only) | .NET Core / .NET 6+ (Cross-Platform) |
| **Binary Name** | `powershell.exe` | `pwsh` (or `pwsh.exe`) |
| **Path Traversal** | Windows backslash (`\`) | Portable forward slash (`/`) and backslash |
| **Parallel Loops** | Requires `Workflow` or runspaces | Native `ForEach-Object -Parallel` |
| **Ternary Operators** | Not supported (`if/else` required) | Supported: `$status = $ok ? 'Pass' : 'Fail'` |
| **Null-Coalescing** | Not supported | Supported: `$val = $input ?? 'Default'` |

---

## 2. Installing and Running `pwsh` on Linux & Docker

### 2.1 Installing on Ubuntu 22.04 LTS
```bash
# Update package repo and install prerequisites
sudo apt-get update && sudo apt-get install -y wget apt-transport-https software-properties-common

# Download Microsoft repository GPG keys
wget -q "https://packages.microsoft.com/config/ubuntu/22.04/packages-microsoft-prod.deb"
sudo dpkg -i packages-microsoft-prod.deb
rm packages-microsoft-prod.deb

# Install PowerShell Core
sudo apt-get update && sudo apt-get install -y powershell

# Verify version
pwsh --version
```

### 2.2 Minimal Docker Container for Cloud Automation
Using official Microsoft Container Registry (`mcr.microsoft.com`) images:

```dockerfile
# Multi-stage production container for CI/CD runners
FROM mcr.microsoft.com/powershell:lts-ubuntu-22.04 AS runtime

# Set non-interactive environment
ENV DEBIAN_FRONTEND=noninteractive \
    POWERSHELL_TELEMETRY_OPTOUT=1

# Install curl, git, and jq for hybrid utilities
RUN apt-get update && apt-get install -y --no-install-recommends \
    curl \
    git \
    jq \
    ca-certificates \
    && rm -rf /var/lib/apt/lists/*

# Install Az and AWS PowerShell modules into system path
RUN pwsh -NoLogo -NoProfile -Command " \
    \$ProgressPreference = 'SilentlyContinue'; \
    Install-Module -Name Az.Accounts -Force -Scope AllUsers; \
    Install-Module -Name Az.Resources -Force -Scope AllUsers; \
    Install-Module -Name AWS.Tools.Common -Force -Scope AllUsers; \
    Install-Module -Name AWS.Tools.S3 -Force -Scope AllUsers; \
"

WORKDIR /workspace
ENTRYPOINT ["pwsh", "-NoLogo", "-NoProfile"]
```

---

## 3. Cross-Platform Interoperability and POSIX Command Execution

When executing native Linux binaries inside `pwsh`, PowerShell captures their output as an array of `System.String` objects rather than streaming raw byte buffers.

```powershell
# Interacting with native Linux tools
$ipOutput = ip -j addr show eth0 | ConvertFrom-Json
$ipAddress = $ipOutput.addr_info | Where-Object family -eq 'inet' | Select-Object -ExpandProperty local

Write-Host "Resolved Linux IPv4: $ipAddress" -ForegroundColor Green
```

### Portable File Path Best Practices
Always use standard forward slashes (`/`) or `[System.IO.Path]::Combine()` to ensure cross-platform compatibility:

```powershell
# Portable Path Construction
$configDir = [System.IO.Path]::Combine([System.Environment]::GetFolderPath('UserProfile'), '.config', 'myapp')
$configFile = Join-Path -Path $configDir -ChildPath 'settings.json'

if (-not (Test-Path -Path $configFile)) {
    New-Item -ItemType File -Path $configFile -Force | Out-Null
}
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 01 - PowerShell Core Architecture and Object Pipeline](./01-PowerShell-Core-Architecture-and-Object-Pipeline.md) | [Index](../../../README.md) | [03 - Advanced Functions Script Blocks and Modules →](./03-Advanced-Functions-Script-Blocks-and-Modules.md) |
