# 13 - Quick Revision & 5-Minute Cheat Sheet

Keep this sheet bookmarked for rapid pre-interview revision and emergency on-call reference.

---

## ⚡ 1-Liner Directory Directory Summary

| Directory | Key Role (In 1 Sentence) |
|---|---|
| **`/`** | The root origin of the unified inverted Linux filesystem hierarchy. |
| **`/bin`** | Essential user command binaries (symlinked to `/usr/bin` in modern distros). |
| **`/sbin`** | Essential system administrator / root binaries (symlinked to `/usr/sbin`). |
| **`/boot`** | Static bootloader files, GRUB configs, kernel images (`vmlinuz`), and `initramfs`. |
| **`/dev`** | Device file nodes for hardware and virtual devices (`/dev/sda`, `/dev/null`). |
| **`/etc`** | Host-specific plain-text configuration files and systemd service units. |
| **`/home`** | Personal directories for regular non-root users (`/home/username`). |
| **`/root`** | Home directory for the superuser `root` (kept on `/` for disaster recovery). |
| **`/lib`** | Shared libraries and kernel modules needed by `/bin` and `/sbin` binaries. |
| **`/usr`** | Secondary hierarchy for user binaries, shared libraries, and documentation. |
| **`/usr/local`**| Target directory for software compiled locally from source by the sysadmin. |
| **`/opt`** | Add-on, self-contained third-party application software packages. |
| **`/var`** | Variable files that grow continuously at runtime (logs, mail, docker layers). |
| **`/tmp`** | Ephemeral temporary scratch space (typically tmpfs in RAM; wiped on reboot). |
| **`/run`** | Volatile early-boot runtime state, PID files, and Unix domain sockets. |
| **`/proc`** | In-memory pseudo-filesystem exposing active process states and live kernel metrics. |
| **`/sys`** | In-memory pseudo-filesystem exposing hardware buses, drivers, and device classes. |
| **`/srv`** | Site-specific data served by this system (web, FTP, version control). |
| **`/mnt`** | Mount point reserved for sysadmins to manually mount temporary filesystems. |
| **`/media`** | Automount directory for pluggable removable media (USB sticks, optical drives). |

---

## ⚖️ Confusing Pairs: At a Glance

| Pair | Distinction |
|---|---|
| **`/` vs `/root`** | `/` is the top-level parent of everything; `/root` is the private home folder of the admin user. |
| **`/bin` vs `/sbin`** | `/bin` is for everyday user commands; `/sbin` is for administrative/maintenance commands. |
| **`/etc` vs `/var`** | `/etc` holds static configuration; `/var` holds dynamic, continuously mutating runtime data. |
| **`/tmp` vs `/var/tmp`**| `/tmp` is wiped on reboot (tmpfs); `/var/tmp` is stored on disk and persists across reboots. |
| **`/opt` vs `/usr/local`**| `/opt` is for bundled 3rd-party apps; `/usr/local` is for software compiled from source. |
| **`/mnt` vs `/media`** | `/mnt` is for manual admin mounts; `/media` is for automounted removable media. |

---

## 🛠️ High-Frequency Diagnostic Commands

```bash
# Check filesystem disk capacity:
df -h

# Check filesystem inode usage (look for 100% full inodes):
df -i

# Find top 10 largest directories in /var:
du -h --max-depth=1 /var | sort -hr | head -n 10

# Find files larger than 500 MB:
find / -type f -size +500M 2>/dev/null

# Find open deleted files holding disk space:
lsof +L1

# Zero-out an active log file without restarting the service:
: > /var/log/active_app.log

# Test /etc/fstab without rebooting:
sudo mount -a

# Inspect live kernel parameters:
cat /proc/sys/net/ipv4/ip_forward
```

---

## 🚦 Disk-Full Emergency Triage Flowchart

```
Server reports: "No space left on device"
                    │
                    ▼
           Run: 'df -h'
                    │
       ┌────────────┴────────────┐
       ▼                         ▼
Disk blocks 100% full?    Disk blocks show FREE space?
       │                         │
       ▼                         ▼
Run 'du -h --max-depth=1'   Run 'df -i' (Check Inodes)
to find big folders.             │
       │                         ├── Inodes 100% full:
       ▼                         │   Find directories with millions of tiny files:
Deleted file with 'rm'           │   'find / -xdev -printf "%h\n" | sort | uniq -c'
and space didn't free?          │   Purge with: 'find ... -type f -delete'
       │                         │
       ▼                         └── Inodes NOT full:
Run 'lsof +L1' to find               Check for hidden files shadowed under mount points:
open unlinked file descriptors.      Unmount suspect mount points and check underlying dir.
Zero via /proc/[PID]/fd/[FD].
```
---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [12 - MCQ](./12-MCQ.md) | [README](./README.md) | [14 - Related Topics](./14-Related-Topics.md) |
