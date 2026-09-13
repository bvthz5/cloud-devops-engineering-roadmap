# 16 - Interview Q&A: Linux Folder Structure & FHS

Frequently asked interview questions for Linux Systems Engineers, DevOps Engineers, and Site Reliability Engineers (SREs).

---

### Q1: What is the difference between `/` and `/root` in Linux?
**Answer:**
- **`/` (Root Directory):** The absolute top-level directory of the entire Linux directory hierarchy. All directories, physical disks, and virtual filesystems stem from `/`.
- **`/root`:** The home directory of the `root` administrative superuser account (equivalent to `/home/username` for standard users).

---

### Q2: What is the Filesystem Hierarchy Standard (FHS)?
**Answer:**
The FHS is a standard specification defining the layout and directory structure of Linux operating systems. It ensures software packages, SysAdmins, and scripts can locate files consistently regardless of distribution (Ubuntu, RHEL, Arch, Debian).

---

### Q3: What are `/proc` and `/sys`, and why do they take 0 bytes on disk?
**Answer:**
`/proc` (procfs) and `/sys` (sysfs) are **virtual pseudo-filesystems** generated dynamically in RAM by the kernel. They do not exist on physical storage drives. They provide a file-based interface to inspect kernel runtime state, process metrics, hardware topology, and driver parameters.

---

### Q4: Explain the modern Linux "usr-merge" architecture.
**Answer:**
Usr-merge consolidates root software directories (`/bin`, `/sbin`, `/lib`, `/lib64`) into symbolic links pointing directly to `/usr/bin`, `/usr/sbin`, `/usr/lib`, and `/usr/lib64`. This simplifies package maintenance, enables read-only `/usr` OS snapshots, and eliminates duplication while maintaining backward compatibility for legacy scripts.

---

### Q5: What is the difference between `/tmp` and `/run`?
**Answer:**
- **`/tmp`:** Intended for general temporary files created by user applications and scripts. It has global `1777` permissions (Sticky Bit enabled) and is cleared periodically or on reboot.
- **`/run`:** A RAM-backed `tmpfs` directory reserved for system runtime state data since the last boot (PID files, sockets, locks). It requires administrative permissions to write.

---

### Q6: Why is `/var/log` often placed on a separate partition in production servers?
**Answer:**
If `/var/log` shares the same partition as the root directory (`/`), a sudden flood of unrotated log entries can consume 100% of disk space. This would crash the entire operating system, prevent SSH logins, and halt database services. Placing `/var/log` on an isolated volume confines disk exhaustion to logs without crashing the host OS.
---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [15 - Troubleshooting](./15-Troubleshooting.md) | [README](./README.md) | [17 - Hands On Practice](./17-Hands-On-Practice.md) |
