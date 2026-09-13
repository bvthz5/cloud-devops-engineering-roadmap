# 07 - Kernel Subsystem: Virtual Filesystem (VFS) & Storage

The Linux kernel storage subsystem allows applications to interact with hard disks, flash NVMe SSDs, network filesystems (NFS), and virtual memory filesystems using an identical, standardized API.

---

## 1. The Virtual Filesystem (VFS) Architecture

The **VFS** provides an object-oriented abstraction layer inside the kernel. It defines four primary object types:

```
┌─────────────────────────────────────────────────────────────┐
│                    VFS OBJECT MODEL                         │
├─────────────────┬───────────────────────────────────────────┤
│ 1. Superblock   │ Represents an entire mounted filesystem.  │
│                 │ Stores filesystem size, block size, and   │
│                 │ status flags.                             │
├─────────────────┼───────────────────────────────────────────┤
│ 2. Inode        │ Represents a unique file or directory.    │
│                 │ Stores file metadata, permissions, and    │
│                 │ block location pointers.                  │
├─────────────────┼───────────────────────────────────────────┤
│ 3. Dentry       │ Directory Entry. Links a human-readable   │
│                 │ filename string to an inode number.       │
│                 │ Cached in RAM via the `dcache`.           │
├─────────────────┼───────────────────────────────────────────┤
│ 4. File Object  │ Represents a file opened by a process.    │
│                 │ Tracks current file offset (read cursor)  │
│                 │ and access mode (`O_RDONLY`, `O_WRONLY`). │
└─────────────────┴───────────────────────────────────────────┘
```

---

## 2. The Dentry Cache (`dcache`)

Resolving an absolute path like `/var/log/nginx/access.log` requires traversing multiple directory inodes. If every directory traversal required reading disk blocks, filesystem operations would be cripplingly slow.

The kernel maintains the **`dcache` (Dentry Cache)** in RAM:
- When a path component is looked up, the resulting `dentry` struct is kept in RAM.
- Subsequent accesses to `/var/log/nginx/access.log` resolve instantly in memory without touching physical storage blocks.

---

## 3. The Block Layer & I/O Schedulers

Between the filesystem drivers (ext4, XFS) and the physical hardware drivers (NVMe, SATA) sits the **Block Layer**. It organizes data transfers into **`bio` (Block I/O)** request queues.

### The Role of the I/O Scheduler:
1. **Merging:** Merges requests for adjacent disk sectors into a single large I/O request.
2. **Sorting:** Reorders requests to minimize mechanical seek head movement on spinning disks.

### Modern Linux I/O Schedulers:
- **`none`:** Direct hardware pass-through. Used exclusively on modern **NVMe SSDs** because NVMe drives support up to 64,000 independent hardware queues directly in silicon!
- **`mq-deadline`:** Enforces strict deadlines on I/O requests to prevent starvation; ideal for enterprise SATA SSDs.
- **`bfq` (Budget Fair Queueing):** Prioritizes desktop interactivity and interactive applications.

```bash
# Check the active I/O scheduler for disk sda:
$ cat /sys/block/sda/queue/scheduler
[mq-deadline] none bfq

# Check for NVMe drive:
$ cat /sys/block/nvme0n1/queue/scheduler
[none] mq-deadline
```
---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [06 - Memory Management](./06-Memory-Management.md) | [README](./README.md) | [08 - Device Drivers and Kernel Modules](./08-Device-Drivers-and-Kernel-Modules.md) |
