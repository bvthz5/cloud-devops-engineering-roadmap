# 10 - Technical Interview Questions & Answers (Junior to Senior/SRE)

---

### Q1: What is the difference between `/` and `/root` in Linux?
**Answer:**
- **`/` (Root Directory):** The top-level ancestor and parent of the entire Linux filesystem hierarchy. Every single file, directory, and mount point branches off from `/`.
- **`/root` (Root User's Home Directory):** The dedicated personal home directory for the superuser administrator `root`. It is intentionally placed directly on the root partition rather than inside `/home/root` so that the administrator can still log in and troubleshoot even if `/home` fails to mount (e.g., during filesystem corruption or network mount failures).

---

### Q2: Why are `/bin`, `/sbin`, and `/lib` symlinked to `/usr/` in modern Linux distributions (UsrMerge)?
**Answer:**
Historically in the 1970s, Unix split binaries between `/bin` and `/usr/bin` because storage disks (RK05 packs) were limited to 1.5 MB. System recovery tools stayed on the root disk in `/bin`, while user tools lived on a second disk mounted at `/usr/bin`.
With modern Linux, `initramfs` pre-loads storage drivers into RAM before mounting the root disk, rendering the split obsolete. Merging them into `/usr` (`UsrMerge`):
1. Enables atomic system upgrades and snapshotting (the whole OS lives cleanly in `/usr`).
2. Simplifies package maintenance by eliminating arbitrary decisions on whether an executable belongs in `/bin` or `/usr/bin`.
3. Makes read-only immutable OS designs (like Fedora Silverblue, Talos) practical.

---

### Q3: Explain the difference between `/proc`, `/sys`, and `/dev`.
**Answer:**
All three are virtual/pseudo filesystems populated dynamically by the kernel rather than stored as physical files on disk:
- **`/dev` (devtmpfs):** Represents hardware devices and kernel interfaces as file nodes (block devices like `/dev/nvme0n1`, character devices like `/dev/tty`, and pseudo-devices like `/dev/null`). It allows userland software to communicate with device drivers using standard I/O calls.
- **`/proc` (procfs):** Exposes process-centric runtime state and general kernel data structures. It contains numeric directories for every running PID (`/proc/[PID]`) and live system statistics (`/proc/meminfo`, `/proc/cpuinfo`).
- **`/sys` (sysfs):** Exposes an organized, tree-like view of hardware devices, buses, drivers, and kernel subsystems (e.g., network interfaces in `/sys/class/net/`, block devices in `/sys/block/`). It is strictly structured and used for hardware management and power states.

---

### Q4: You deleted a 40 GB log file using `rm /var/log/app.log`, but `df -h` still reports the disk as 100% full. Why, and how do you resolve it without restarting the server?
**Answer:**
- **Cause:** In Linux, running `rm` only unlinks the filename from the directory entry. If a running process (like a web server or daemon) still holds an open file descriptor to that file, the kernel will not release the underlying disk blocks until that process closes the descriptor or terminates.
- **Troubleshooting:**
  1. Identify the holding process using `lsof | grep deleted` or `lsof +L1`.
  2. Note the Process ID (PID) and the File Descriptor (FD) number (e.g., PID 5120, FD 3).
- **Resolution:**
  - If the service can be safely reloaded, run `systemctl reload <service>`.
  - If zero downtime is required and restarting is prohibited, truncate the open file descriptor directly through procfs:
    ```bash
    truncate -s 0 /proc/5120/fd/3
    # or
    : > /proc/5120/fd/3
    ```
    This immediately flushes the allocated disk blocks to 0 bytes while allowing the service to continue running uninterrupted.

---

### Q5: A Linux server reports "No space left on device", but `df -h` shows 50% available disk capacity. What is wrong?
**Answer:**
- **Cause:** The filesystem has suffered **Inode Exhaustion**.
  While physical storage blocks are available, all allocated index nodes (inodes) have been used up. This commonly happens when an application creates millions of tiny 0-byte or 1-byte files (such as PHP session files, postfix mail queues, or cache tokens).
- **Diagnosis:** Run `df -i` to check inode usage (`IUse%` will show 100%).
- **Resolution:**
  Use `find` to discover the offending directory:
  ```bash
  find /var -xdev -printf '%h\n' | sort | uniq -c | sort -rn | head -n 10
  ```
  Then delete the files using `find ... -delete` (since `rm *` will fail with "Argument list too long").

---

### Q6: What does an Inode contain, and what does it NOT contain?
**Answer:**
- **An Inode contains:**
  - File type (regular file, directory, symlink, socket, FIFO, device)
  - Permissions (`rwx` for owner, group, others)
  - Ownership (UID, GID)
  - File size in bytes
  - Timestamps (`atime`, `mtime`, `ctime`, and `crtime`/`btime` where supported)
  - Hard link count
  - Pointers to disk data blocks
- **An Inode does NOT contain:**
  1. The **File Name**.
  2. The **File Path**.
  Both the name and path are stored as entries inside a parent directory file, which maps string names to inode numbers.

---

### Q7: What is the `nofail` mount option in `/etc/fstab`, and why is it critical in Cloud environments (AWS/GCP/Azure)?
**Answer:**
By default, if an entry in `/etc/fstab` cannot be mounted during the boot process (e.g., an external EBS volume is detached, corrupted, or delayed), systemd will halt the system boot and drop into an interactive maintenance/emergency shell.
Because cloud instances are headless and have no physical monitor or keyboard, dropping into an emergency shell means the VM will fail health checks and become completely unreachable over SSH.
Adding the `nofail` option (e.g., `defaults,nofail`) instructs the OS to continue booting normally even if that specific filesystem cannot be mounted.

---

### Q8: What is the difference between `/tmp` and `/var/tmp`?
**Answer:**
- **`/tmp`:** Designed for short-lived temporary files. On modern systemd Linux, it is mounted as a volatile `tmpfs` (in RAM) and is completely erased on every reboot. Automated cleaners also delete files older than 10 days.
- **`/var/tmp`:** Designed for persistent temporary files that must survive across system reboots. It is stored on physical disk and cleaners typically retain files for up to 30 days.

---

### Q9: Why is the current directory `.` not included in the `$PATH` environment variable by default?
**Answer:**
It is a critical security protection. If `.` were in `$PATH`, an attacker with write access to a shared directory (like `/tmp`) could place a malicious executable named `ls`, `cd`, or `cat`.
If an administrator navigating through `/tmp` typed `ls`, the shell would execute the attacker's script instead of `/usr/bin/ls`, resulting in immediate privilege compromise. By omitting `.`, users are forced to explicitly type `./script` to run local executables.

---

### Q10: What is a Bind Mount, and how is it used in container environments like Docker?
**Answer:**
A bind mount makes an existing directory or file available at a second mount point elsewhere in the filesystem tree, bypassing standard symlinks. Both mount points reference the exact same underlying inodes and disk blocks.
In Docker, bind mounts (`docker run -v /host/path:/container/path`) allow a container to mount a directory directly from the host operating system into its private mount namespace, enabling shared configuration files, persistent data storage, or live source code reloading.
---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [09 - Troubleshooting](./09-Troubleshooting.md) | [README](./README.md) | [11 - Hands On Practice](./11-Hands-On-Practice.md) |
