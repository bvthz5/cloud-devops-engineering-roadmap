# 14 - Real-World Scenarios & Production Partitioning

In enterprise production environments (AWS EC2, Kubernetes Nodes, Bare-Metal RHEL/Ubuntu Servers), putting the entire filesystem onto a single partition (`/`) is considered a major anti-pattern.

---

## 🏗️ 1. Standard Production Partitioning Layout

| Mount Point | Recommended Partition Size | Rationale |
| :--- | :--- | :--- |
| **`/`** | 20 GB - 50 GB | Holds OS binaries (`/usr`), basic config (`/etc`). |
| **`/boot`** | 1 GB - 2 GB | Holds GRUB and kernel images. Isolated from disk fill-ups. |
| **`/var`** | 50 GB - 500 GB+ | Separate LVM volume for logs (`/var/log`) and databases (`/var/lib`). |
| **`/tmp`** | 5 GB - 10 GB | Mounted as `tmpfs` (RAM) or isolated partition with `noexec,nosuid` flags. |
| **`/home`** | Variable | Keeps user files separate from OS reinstalls or upgrades. |
| **`/data`** | Enterprise SAN / EBS | Dedicated multi-TB storage volume for application workloads. |

---

## 🚨 2. Real-World Outage: The 100% `/var/log` Disk Exhaustion

### Scenario:
An Nginx web server experiences a Denial-of-Service (DoS) attack, generating gigabytes of access logs every minute. `/var/log/nginx/access.log` consumes all available disk space on `/`.

### Consequence:
- System services cannot write PID files or sockets to `/run`.
- SSH authentication (`sshd`) fails because it cannot append to `/var/log/auth.log`.
- Database transactions freeze due to uncommitted WAL log writes.

### Production Solution:
1. **Partition Isolation:** Mount `/var/log` on its own dedicated LVM volume so a 100% full log partition never crashes root (`/`).
2. **Automated Log Rotation (`logrotate`):** Configure compression (`gzip`) and strict retention limits (e.g., max 7 days of logs).
3. **External Log Forwarding:** Stream logs out immediately via Fluentd/Promtail to Grafana Loki or AWS CloudWatch.

---

## 🛡️ 3. Security Hardening with Mount Options

In high-security environments (CIS Benchmarks, PCI-DSS compliance), administrators mount specific directories with restrictive security flags in `/etc/fstab`:

```text
# Example /etc/fstab entry for /tmp and /var/tmp hardening
tmpfs    /tmp        tmpfs    defaults,rw,nosuid,nodev,noexec    0 0
```

- **`noexec`:** Prevents execution of binary executables inside `/tmp` (stops malicious scripts downloaded to `/tmp` from executing).
- **`nosuid`:** Disables SUID bit execution inside the directory.
- **`nodev`:** Disables block/character device node creation.

---

## ⬅️ Navigation
- Previous: [13 - Useful Inspection Commands](file:///c:/Users/binil/OneDrive/Desktop/cloud-devops-engineering-roadmap/Study_Notes_System/akumen's/Linux/06-Linux-Folder-Structure/13-Useful-Commands.md)
- Next: [15 - Troubleshooting Filesystem Issues](file:///c:/Users/binil/OneDrive/Desktop/cloud-devops-engineering-roadmap/Study_Notes_System/akumen's/Linux/06-Linux-Folder-Structure/15-Troubleshooting.md)
