# 12 - Filesystem Hierarchy Standard (FHS) Mental Model

The **Filesystem Hierarchy Standard (FHS)** classifies all Linux directories along two functional axes:
1. **Static vs. Variable**
2. **Shareable vs. Unshareable**

Understanding this 2x2 matrix provides a clear mental model for system design, NFS network exports, read-only root mounts, and backup policies.

---

## 🧩 The FHS Categorization Matrix

| | **Shareable** (Can be shared over NFS to other hosts) | **Unshareable** (Must remain local to this specific machine) |
| :--- | :--- | :--- |
| **Static** (Infrequent edits; binaries/docs) | `/usr`, `/opt`, `/usr/share` | `/boot`, `/etc`, `/lib` |
| **Variable** (Continuous writes; logs/spools) | `/srv`, `/var/mail`, `/var/www` | `/var/log`, `/var/run`, `/run`, `/tmp`, `/proc` |

---

## 📐 Detailed Breakdown of Categories

### 1. Static vs Variable
- **Static Files/Directories:** Do not change without explicit SysAdmin intervention (package upgrades or source compilation). Examples: `/usr/bin`, `/boot`, `/etc`.
- **Variable Files/Directories:** Change constantly during system execution without admin action. Examples: `/var/log`, `/tmp`, `/var/spool/mail`.

### 2. Shareable vs Unshareable
- **Shareable Files/Directories:** Architecture-independent files that can safely be mounted across multiple network workstations via NFS or SMB. Examples: `/usr/share/man`, `/srv/www`, `/home`.
- **Unshareable Files/Directories:** Host-specific configurations, hardware nodes, or runtime states tied to one physical machine instance. Examples: `/etc/fstab`, `/boot/vmlinuz`, `/var/log/syslog`, `/proc`.

---

## 💡 Practical DevOps Application

By understanding the FHS matrix:
- Engineers mount `/var/log` on local NVMe storage for maximum logging write speeds.
- Engineers export `/srv/www` or `/home` over NFS for central web server fleets.
- Engineers mount `/usr` as Read-Only (`ro`) in container security hardening profiles to prevent malware from modifying system binaries.
---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [11 - Symbolic Links bin sbin lib](./11-Symbolic-Links-bin-sbin-lib.md) | [README](./README.md) | [13 - Useful Commands](./13-Useful-Commands.md) |
