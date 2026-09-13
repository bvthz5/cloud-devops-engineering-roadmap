# 08 - Volatile & Temporary Data: `/tmp` vs `/run`

Both `/tmp` and `/run` handle temporary runtime files, but they differ significantly in lifetime, accessibility, security, and underlying filesystem backing.

---

## ⚡ 1. The `/tmp` Directory (Temporary Files)

`/tmp` stores temporary files created by user applications, compilers, and scripts during execution.

### Key Characteristics of `/tmp`
- **Global Write Access with Sticky Bit:** `/tmp` has octal permission `1777` (`rwxrwxrwt`). The **Sticky Bit (`t`)** ensures any user can create files, but users can only delete or modify files they own.
- **Cleared Automatically:** Content in `/tmp` is wiped automatically upon system reboot or periodically by systemd services (`systemd-tmpfiles-clean.service`).
- **Disk or RAM Backed:** In modern distros, `/tmp` is often mounted as a RAM-backed `tmpfs` virtual filesystem for ultra-fast I/O performance.

---

## 🏃 2. The `/run` Directory (Runtime Variable Data)

Introduced in modern Linux distributions (replacing the legacy `/var/run`), `/run` describes system state data for running daemons since the last boot.

### Key Characteristics of `/run`
- **Volatile `tmpfs` RAM Filesystem:** Mounted in RAM on boot; cleared completely on shutdown or reboot.
- **Restricted Access:** Owned by `root:root` (`755`). Only processes with appropriate privileges can write here.
- **Stores Process Handles & Sockets:**
  - PID files (`/run/nginx.pid` holding service Process IDs).
  - UNIX Domain Sockets (`/run/docker.sock`, `/run/dbus/system_bus_socket`).
  - Lock files (`/run/lock/`) preventing dual execution of applications.

---

## 📊 Summary Comparison: `/tmp` vs `/run`

| Attribute | `/tmp` | `/run` (formerly `/var/run`) |
| :--- | :--- | :--- |
| **Purpose** | Scratch space for user applications & scripts. | System runtime state (PIDs, sockets, locks). |
| **Permissions** | `1777` (`rwxrwxrwt`) - World writable + Sticky bit. | `0755` (`rwxr-xr-x`) - Admin/Service write only. |
| **Persistence** | Cleared periodically or on reboot. | Cleared completely on every reboot (`tmpfs`). |
| **Example Files** | `compilation_cache.tmp`, `upload_xyz.tmp` | `docker.sock`, `sshd.pid`, `lock/` |

---

## 🛠️ Inspecting Temporary Mounts

```bash
# Check if /tmp or /run are mounted on tmpfs (RAM)
df -hT | grep tmpfs

# Inspect sticky bit permission on /tmp
ls -ld /tmp
# Output: drwxrwxrwt 12 root root ... /tmp
```
---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [07 - opt and srv](./07-opt-and-srv.md) | [README](./README.md) | [09 - proc sys and dev](./09-proc-sys-and-dev.md) |
