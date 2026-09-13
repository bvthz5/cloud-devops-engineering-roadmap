# 17 - Hands-On Practice & Terminal Labs: Linux Folder Structure

Reinforce your understanding of the Linux directory hierarchy with these practical terminal exercises.

---

## 🧪 Lab 1: Inspecting the Top-Level Directory Tree

1. Open your terminal and list all directories under `/` with long format:
   ```bash
   ls -ld /*
   ```
2. Identify which directories are symbolic links pointing to `/usr` (e.g., `/bin`, `/sbin`, `/lib`).
3. Check the total number of items inside `/proc` (including process folders):
   ```bash
   ls -d /proc/[0-9]* | wc -l
   ```

---

## 🧪 Lab 2: Investigating Virtual Filesystems (`procfs` & `sysfs`)

1. Read your CPU information directly from the kernel:
   ```bash
   cat /proc/cpuinfo | grep "model name" | head -n 1
   ```
2. Check system uptime in seconds from `/proc/uptime`:
   ```bash
   cat /proc/uptime
   ```
3. List active block storage devices exposed in `/sys`:
   ```bash
   ls -l /sys/block/
   ```

---

## 🧪 Lab 3: Disk Space & Mount Analysis

1. Use `findmnt` to display the hierarchy of mounted filesystems:
   ```bash
   findmnt --real
   ```
2. Find the 5 largest subdirectories inside `/var` using `du`:
   ```bash
   sudo du -h --max-depth=1 /var 2>/dev/null | sort -hr | head -n 5
   ```
3. Create a temporary mount point directory under `/mnt`:
   ```bash
   sudo mkdir -p /mnt/practice_lab
   ls -ld /mnt/practice_lab
   sudo rmdir /mnt/practice_lab
   ```
---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [16 - Interview QA](./16-Interview-QA.md) | [README](./README.md) | [18 - MCQ](./18-MCQ.md) |
