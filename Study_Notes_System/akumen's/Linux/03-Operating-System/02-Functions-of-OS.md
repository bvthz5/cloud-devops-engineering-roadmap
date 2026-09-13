# 02 - Core Functions of an Operating System

An operating system acts as the central coordinator of the computing environment. Its responsibilities are categorized into seven major functional areas.

---

## 🏛️ The 7 Core Functions

```
┌────────────────────────────────────────────────────────────────────────┐
│                        CORE FUNCTIONS OF AN OS                         │
├────────────────────────────────┬───────────────────────────────────────┤
│ 1. Processor (CPU) Management  │ Process scheduling, threads, context  │
│                                │ switching, CPU time slicing.          │
├────────────────────────────────┼───────────────────────────────────────┤
│ 2. Memory Management           │ RAM allocation, virtual memory,       │
│                                │ paging, swapping, memory protection.  │
├────────────────────────────────┼───────────────────────────────────────┤
│ 3. File & Storage Management   │ Filesystem hierarchies, directories,  │
│                                │ inodes, access control permissions.   │
├────────────────────────────────┼───────────────────────────────────────┤
│ 4. Device & I/O Management     │ Device drivers, interrupt handling,   │
│                                │ buffering, caching, DMA transfers.    │
├────────────────────────────────┼───────────────────────────────────────┤
│ 5. Security & Protection       │ User authentication, access control   │
│                                │ (DAC/MAC), process isolation, cgroups.│
├────────────────────────────────┼───────────────────────────────────────┤
│ 6. Job Accounting & Metrics    │ Resource usage tracking, process time,│
│                                │ auditing logs, performance metrics.   │
├────────────────────────────────┼───────────────────────────────────────┤
│ 7. Error Detection & Recovery  │ Hardware fault handling, signal       │
│                                │ delivery, core dumps, kernel panics.  │
└────────────────────────────────┴───────────────────────────────────────┘
```

---

## 🔬 Deep Dive: Function-by-Function

### 1. Processor (CPU) Management
- **The Challenge:** Multiple processes want to compute at the same time, but physical CPU cores are limited.
- **The OS Solution:** The OS maintains a queue of runnable tasks. It uses an algorithm (such as Linux's **Completely Fair Scheduler - CFS**) to decide which process gets CPU time, for how long (quantum), and when it must be preempted.

### 2. Memory Management
- **The Challenge:** Programs have unpredictable memory footprints, and rogue processes must not corrupt memory allocated to other applications.
- **The OS Solution:** The OS tracks every byte of physical memory (allocated vs free), divides RAM into standard 4 KB **pages**, assigns independent **virtual address spaces** to each process, and swaps idle pages to disk (Swap) when RAM runs low.

### 3. File Management
- **The Challenge:** Raw storage media consists of unstructured magnetic sectors or flash blocks.
- **The OS Solution:** The OS provides a structured filesystem (such as ext4 or XFS). It organizes data into human-readable directories, tracks file metadata via inodes, enforces read/write/execute permissions, and guarantees consistency using journaling.

### 4. Device & I/O Management
- **The Challenge:** Devices communicate at wildly different speeds (a keyboard transmits bytes per second, while an NVMe SSD transfers gigabytes per second).
- **The OS Solution:** 
  - **Buffering:** Storing data temporarily in RAM to match speed differences between sender and receiver.
  - **Caching:** Keeping copies of frequently accessed data in fast memory.
  - **Spooling:** Queuing print jobs or background data transfers for sequential output.
  - **Device Drivers:** Providing standardized software adapters for diverse hardware vendors.

### 5. Security & Protection
- **The Challenge:** Multi-user and multi-tenant environments require strict boundaries to prevent unauthorized data access or privilege escalation.
- **The OS Solution:** The OS enforces Discretionary Access Control (**DAC** via file permissions `chmod`), Mandatory Access Control (**MAC** via SELinux/AppArmor), and resource limits (**cgroups**).

---

## 🛠️ OS Functions to Linux Implementation Mapping

| Theoretical OS Function | Linux Subsystem / Implementation | Linux Admin Command / Metric |
|---|---|---|
| **CPU Scheduling** | Completely Fair Scheduler (CFS), `task_struct` | `top`, `htop`, `nice`, `renice`, `chrt` |
| **Memory Allocation** | Virtual Memory Manager, Page Cache, MMU | `free -m`, `vmstat`, `/proc/meminfo` |
| **Filesystem Management**| Virtual Filesystem (VFS), ext4/XFS drivers | `ls`, `df -h`, `mount`, `tune2fs` |
| **I/O Management** | Block Layer, DMA engines, `udev` daemon | `iostat`, `lsblk`, `iotop`, `/dev/` |
| **Access Control** | UID/GID, POSIX ACLs, SELinux, AppArmor | `chmod`, `chown`, `getfacl`, `sestatus` |
| **Resource Accounting** | Process Accounting (`psacct`), Cgroups v2 | `ps`, `systemd-cgtop`, `/sys/fs/cgroup` |
| **Fault Recovery** | Signals (`SIGSEGV`, `SIGKILL`), Kernel Panic | `dmesg`, `journalctl`, `coredumpctl` |
---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [01 - OS Basics](./01-OS-Basics.md) | [README](./README.md) | [03 - Process Management](./03-Process-Management.md) |
