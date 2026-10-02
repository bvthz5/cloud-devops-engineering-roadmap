# 17 - Quick Revision & 5-Minute Kernel Cheat Sheet

Keep this cheat sheet bookmarked for rapid review of kernel concepts, commands, and memory subsystems.

---

## ⚡ The 4 Core Responsibilities Summary

| Responsibility | Purpose | Primary Data Structure / Subsystem |
|---|---|---|
| **Process Management** | Schedules tasks across CPU cores; context switching | `struct task_struct`, CFS / EEVDF |
| **Memory Management** | Allocates RAM, virtual memory, manages page cache | Buddy Allocator, SLUB, MMU Page Tables |
| **Filesystem Management**| Unifies storage behind a standard POSIX API | Virtual Filesystem (VFS), Inodes, `dentry` |
| **Device Control** | Drives hardware peripherals; services interrupts | Device Drivers, IRQ Handlers, DMA, LKMs |

---

## 💾 Memory Allocator Dual Hierarchy
- **Buddy System:** Manages physical memory in power-of-two multiples of **4 KB pages** (Order 0 = 4 KB ... Order 10 = 4 MB). Prevents external fragmentation.
- **SLUB Allocator:** Carves whole 4 KB pages into dedicated pools of small, fixed-size kernel objects (`task_struct`, `inode`, `dentry`). Prevents internal fragmentation.

---

## 🌳 Virtual Filesystem (VFS) 4 Core Objects
1. **Superblock:** Mounted filesystem metadata and parameters.
2. **Inode:** Unique file metadata record and data block pointers.
3. **Dentry:** Maps a pathname string component to an inode number.
4. **File:** Represents an active file descriptor held open by a process.

---

## 🛠️ High-Frequency Kernel Administration Commands

```bash
# Print kernel release version:
uname -r

# View kernel ring buffer errors and OOM events:
sudo dmesg -T --level=err,crit

# Inspect kernel SLUB object cache allocation:
sudo slabtop -s c

# List loaded kernel modules:
lsmod

# Load module with dependencies:
sudo modprobe <module_name>

# Safely unload module:
sudo modprobe -r <module_name>

# Inspect live tunable kernel parameter:
sysctl net.ipv4.ip_forward

# Modify kernel parameter live in RAM:
sudo sysctl -w net.ipv4.ip_forward=1

# Apply persistent sysctl configuration:
sudo sysctl -p /etc/sysctl.d/99-custom.conf
```

---

## ⚙️ Essential Production `sysctl` Tuning Variables

| Parameter | Recommended Value | Why? |
|---|:---:|---|
| **`net.ipv4.ip_forward`** | `1` | Enables packet routing across interfaces (required for K8s / Docker). |
| **`vm.swappiness`** | `10` | Reduces aggressive swapping; prioritizes keeping app memory in RAM. |
| **`kernel.panic`** | `10` | Automatically reboots host 10 seconds after a kernel panic. |
| **`fs.file-max`** | `2097152` | Raises system-wide file descriptor limit for high-concurrency servers. |
| **`net.core.somaxconn`** | `4096` | Increases TCP listen backlog queue for high-traffic web services. |
---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [16 - MCQ](./16-MCQ.md) | [README](./README.md) | [18 - Related Topics](./18-Related-Topics.md) |
