# 12 - Multiple Choice Questions (Self-Assessment)

Test your understanding of the Linux Directory Structure, FHS, and filesystem mechanics. Each question contains four options and an expandable answer with a detailed explanation.

---

### Q1: In modern Linux distributions, what is the relationship between `/bin` and `/usr/bin`?
- **A)** `/bin` contains system administrator tools, while `/usr/bin` contains user tools.
- **B)** `/bin` is a symbolic link pointing to `/usr/bin` as part of the UsrMerge standard.
- **C)** `/usr/bin` is a backup copy of `/bin` used only in single-user recovery mode.
- **D)** `/bin` is mounted from physical disk, while `/usr/bin` is an in-memory tmpfs.

<details>
<summary>👉 View Answer & Explanation</summary>

**Correct Answer: B**  
**Explanation:** Under modern Linux distributions (Debian, Ubuntu, RHEL, Arch), `/bin`, `/sbin`, and `/lib` are symbolic links pointing to `/usr/bin`, `/usr/sbin`, and `/usr/lib`. This "UsrMerge" eliminates historic disk-space workarounds from the 1970s and enables atomic OS upgrades and snapshots.
</details>

---

### Q2: Which file in `/etc` should you inspect to determine the static filesystem mount configuration applied during system boot?
- **A)** `/etc/mtab`
- **B)** `/etc/fstab`
- **C)** `/etc/filesystems`
- **D)** `/etc/inittab`

<details>
<summary>👉 View Answer & Explanation</summary>

**Correct Answer: B**  
**Explanation:** `/etc/fstab` (File Systems Table) defines all static mount points, device identifiers (UUIDs), filesystem types, and boot mount options. `/etc/mtab` historically showed currently mounted devices, but is now usually a symlink to `/proc/mounts`.
</details>

---

### Q3: What happens to files inside a directory when a new partition is mounted onto that directory?
- **A)** The existing files are permanently deleted.
- **B)** The mount command throws an error and fails if the directory is not empty.
- **C)** The existing files are shadowed and become inaccessible until the partition is unmounted.
- **D)** The existing files are automatically merged into the newly mounted partition.

<details>
<summary>👉 View Answer & Explanation</summary>

**Correct Answer: C**  
**Explanation:** Mounting a filesystem over a non-empty directory shadows the existing contents. The underlying files remain intact on the parent filesystem, but cannot be viewed or accessed until the new partition is unmounted.
</details>

---

### Q4: Which of the following pieces of metadata is NOT stored inside a file's Inode?
- **A)** File permissions (rwx)
- **B)** The file's name
- **C)** File size in bytes
- **D)** Timestamps (atime, mtime, ctime)

<details>
<summary>👉 View Answer & Explanation</summary>

**Correct Answer: B**  
**Explanation:** An inode does not store the file name or file path. Inodes only store attributes, permissions, ownership, timestamps, and block pointers. The filename is stored exclusively as an entry in a directory file mapping the name to an inode number.
</details>

---

### Q5: A Linux server reports "No space left on device", but `df -h` shows 45 GB of free space. Which command should you run immediately to diagnose the issue?
- **A)** `du -sh /`
- **B)** `ls -l /tmp`
- **C)** `df -i`
- **D)** `free -m`

<details>
<summary>👉 View Answer & Explanation</summary>

**Correct Answer: C**  
**Explanation:** The `df -i` command checks inode usage. When a disk has millions of tiny or empty files, it can run out of available inodes (100% inode usage) even though physical disk capacity is still plentiful.
</details>

---

### Q6: If an administrator compiles and installs an open-source tool manually from source code using `make install`, where should it be installed according to FHS?
- **A)** `/opt`
- **B)** `/usr/bin`
- **C)** `/usr/local`
- **D)** `/var/lib`

<details>
<summary>👉 View Answer & Explanation</summary>

**Correct Answer: C**  
**Explanation:** According to the FHS, locally compiled software should be installed into `/usr/local` (`/usr/local/bin`, `/usr/local/lib`) to prevent overwriting vendor packages managed by the system package manager (apt/dnf) in `/usr`.
</details>

---

### Q7: Why is `/dev/null` commonly referred to as the "bit bucket"?
- **A)** It stores deleted files in a recycled state until reboot.
- **B)** It discards all data written to it and returns End-of-File (EOF) when read.
- **C)** It generates an infinite sequence of random bits for encryption.
- **D)** It serves as the primary system swap cache for small files.

<details>
<summary>👉 View Answer & Explanation</summary>

**Correct Answer: B**  
**Explanation:** `/dev/null` is a special character device. Any data written to `/dev/null` is instantly discarded by the kernel. Reading from it immediately returns EOF.
</details>

---

### Q8: What does the first character `b` signify in the output of `ls -l /dev/sda1`?
- **A)** Binary executable
- **B)** Backup file
- **C)** Block device
- **D)** Boot sector partition

<details>
<summary>👉 View Answer & Explanation</summary>

**Correct Answer: C**  
**Explanation:** In `ls -l`, `b` denotes a block device (storage devices like hard drives, SSDs, and NVMe drives that transfer data in buffered fixed-size blocks).
</details>

---

### Q9: Which mount option should be specified in `/etc/fstab` for secondary Cloud EBS/SAN data volumes to prevent the VM from stalling in emergency mode if the volume fails to attach?
- **A)** `noexec`
- **B)** `sync`
- **C)** `nofail`
- **D)** `nodiratime`

<details>
<summary>👉 View Answer & Explanation</summary>

**Correct Answer: C**  
**Explanation:** The `nofail` option ensures systemd will report a warning but continue booting normally if the device is missing or unmountable, preventing catastrophic outages on headless cloud instances.
</details>

---

### Q10: An administrator deleted an active 20 GB log file with `rm`, but `df -h` shows the space was not freed. Which command will identify the process holding the deleted file descriptor open?
- **A)** `ps aux | grep log`
- **B)** `lsof | grep deleted`
- **C)** `fdisk -l`
- **D)** `dmesg | tail`

<details>
<summary>👉 View Answer & Explanation</summary>

**Correct Answer: B**  
**Explanation:** `lsof` (List Open Files) with `grep deleted` or `lsof +L1` lists processes holding open file descriptors for unlinked files, revealing why the kernel has not freed the associated data blocks.
</details>
---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [11 - Hands On Practice](./11-Hands-On-Practice.md) | [README](./README.md) | [13 - Quick Revision](./13-Quick-Revision.md) |
