# 04 - Security, Permissions & System Reliability

Security and operational reliability are foundational architectural advantages of Linux over Windows.

---

## 🔒 1. Granular POSIX Permission Model

Linux enforces strict POSIX file permissions (`rwx` for User, Group, Others) combined with execution flags.
- **No Automatic Execution:** Unlike Windows, where downloading a `.exe` or `.bat` file allows immediate double-click execution, Linux files cannot be executed unless explicitly granted execute permissions (`chmod +x script.sh`).
- **Least Privilege Execution:** Services in Linux run under dedicated unprivileged system users (e.g., `www-data`, `nginx`, `postgres`). If Nginx is compromised, the attacker cannot modify system binaries outside `/var/www` without root privilege escalation.

---

## 🛡️ 2. Minimal Attack Surface & No Vulnerable Registry

- **No Monolithic Registry:** Windows uses a single centralized database (`Registry`) susceptible to malware modification and bloat. Linux configurations are stored in isolated, human-readable text files inside `/etc`.
- **Package Repository Signing:** Linux software is installed from cryptographically signed distribution package repositories (`apt`, `dnf`), minimizing drive-by malware infections common with downloadable `.exe` installers.
- **Security Hardening Modules:** Native kernel security modules like **SELinux** (Security-Enhanced Linux) and **AppArmor** enforce Mandatory Access Control (MAC) policies.

---

## ⌛ 3. Reliability & Zero Forced Reboots

```text
Windows Update Policy:
  - System updates frequently require mandatory OS reboots.
  - Services freeze during update installation restarts.

Linux Live Patching Policy:
  - Software updates (Nginx, Python, Docker) restart only the specific service daemon.
  - Kernel updates can be hot-patched in memory without rebooting (Kpatch, Canonical Livepatch).
```

---

## ⬅️ Navigation
- Previous: [03 - Performance & Efficiency](file:///c:/Users/binil/OneDrive/Desktop/cloud-devops-engineering-roadmap/Study_Notes_System/akumen's/Linux/08-Linux-over-Windows/03-Performance-Efficiency-and-Resource-Usage.md)
- Next: [05 - Linux vs Windows Comparison Matrix](file:///c:/Users/binil/OneDrive/Desktop/cloud-devops-engineering-roadmap/Study_Notes_System/akumen's/Linux/08-Linux-over-Windows/05-Linux-vs-Windows-Comparison-Matrix.md)
