# 08 - Practical Commands & Cross-Platform Equivalents

A comparative guide showing equivalent operations between Linux Bash commands and Windows PowerShell / CMD commands.

---

## 🛠️ Equivalent Command Reference Matrix

| Administrative Operation | Linux Command (Bash) | Windows Equivalent (PowerShell / CMD) |
| :--- | :--- | :--- |
| **List Directory Contents** | `ls -la` | `Get-ChildItem` / `dir` |
| **Print Working Directory** | `pwd` | `Get-Location` / `cd` |
| **Copy Files** | `cp -a source dest` | `Copy-Item -Recurse` / `xcopy` |
| **Move / Rename Files** | `mv old new` | `Move-Item` / `move` |
| **Remove Files / Folders** | `rm -rf dir/` | `Remove-Item -Recurse -Force` / `rmdir /s` |
| **View File Contents** | `cat file.txt` | `Get-Content file.txt` / `type` |
| **Search Text Streams** | `grep "pattern" file` | `Select-String -Pattern "pattern"` / `findstr` |
| **Process Inspection** | `ps aux` / `top` | `Get-Process` / `tasklist` |
| **Terminate Process** | `kill -9 <PID>` | `Stop-Process -Id <PID>` / `taskkill /F /PID` |
| **Network Interfaces** | `ip addr` / `ifconfig` | `Get-NetIPAddress` / `ipconfig` |
| **Test Network Reachability**| `ping -c 4 host` | `Test-Connection host` / `ping host` |
| **Active Network Sockets** | `ss -tulpn` / `netstat` | `Get-NetTCPConnection` / `netstat -ano` |
| **System Uptime** | `uptime` | `(Get-CimInstance Win32_OperatingSystem).LastBootUpTime` |
| **Package Manager** | `apt install pkg` / `dnf` | `winget install pkg` / `choco install` |

---

## ⬅️ Navigation
- Previous: [07 - CLI vs GUI Philosophies](file:///c:/Users/binil/OneDrive/Desktop/cloud-devops-engineering-roadmap/Study_Notes_System/akumen's/Linux/08-Linux-over-Windows/07-CLI-vs-GUI-Philosophies.md)
- Next: [09 - Real-World Production Scenarios](file:///c:/Users/binil/OneDrive/Desktop/cloud-devops-engineering-roadmap/Study_Notes_System/akumen's/Linux/08-Linux-over-Windows/09-Real-World-Production-Scenarios.md)
