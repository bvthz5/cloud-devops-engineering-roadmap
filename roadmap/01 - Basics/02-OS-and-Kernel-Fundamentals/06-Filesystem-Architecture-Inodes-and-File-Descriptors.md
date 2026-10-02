# 06 — Filesystem Architecture, Inodes, and File Descriptors

---

## 1. The Virtual File System (VFS)

The **Virtual File System (VFS)** is the kernel abstraction layer that allows Linux to present a single, unified filesystem hierarchy to user applications, regardless of the underlying physical storage media or filesystem format.

```text
User Space Application (ls, cat, python, nginx)
                      │
                      ▼ Standard Syscalls (open, read, write)
=====================================================
KERNEL VFS LAYER (Unified Interface)
  ├─ Inode Operations    ├─ File Operations
  └─ Dentry Cache        └─ Superblock Management
=====================================================
Underlying Filesystem Drivers:
  ├── ext4 (Local SSD)
  ├── XFS  (High-throughput storage)
  ├── NFS  (Network storage)
  ├── tmpfs (RAM-backed in-memory)
  └── procfs / sysfs (Kernel pseudo-filesystems)
```

The VFS defines four primary object types:
1. **Superblock:** Represents an entire mounted filesystem (filesystem type, size, status, block size).
2. **Inode (Index Node):** Represents the metadata of an individual file or directory.
3. **Dentry (Directory Entry):** Represents a specific path component (e.g., `/usr`, `/bin`), linking human-readable names to inode numbers.
4. **File Object:** Represents an open file instance created when a process invokes `open()`. Holds current file offset and access mode.

---

## 2. Inodes (Index Nodes) Deep Dive

In Unix/Linux, **filenames are not stored inside files**. A file's metadata is stored entirely within an **Inode**.

### What Is Stored in an Inode?
- File size (in bytes).
- Device ID where the file is stored.
- User ID (UID) and Group ID (GID) of the owner.
- File permissions (Read, Write, Execute, SUID, SGID).
- Timestamps:
  - **`atime`:** Access time (last read).
  - **`mtime`:** Modification time (last content write).
  - **`ctime`:** Change time (last metadata or permission change).
- Hard link count (how many filenames point to this inode).
- Pointers to physical disk data blocks storing the file's raw contents.

> **CRITICAL FACT:** The Inode **does NOT store the filename**. The filename and inode number pairing is stored inside a **Directory** (which is just a special file whose contents map names to inode numbers).

```text
Directory Entry (dentry)
Name: "config.yaml" ───► Inode # 240185
                           ├── Size: 4120 bytes
                           ├── Owner: root (UID 0)
                           ├── Perms: rw-r--r-- (0644)
                           ├── Links: 1
                           └── Data Pointers ──► [ Block # 81920 ]
                                                 [ Block # 81921 ]
```

### Inspecting Inodes with `stat` and `ls -i`
```bash
# View inode number of a file
ls -i /etc/hosts

# View complete inode metadata
stat /etc/hosts
```
Sample Output:
```text
  File: /etc/hosts
  Size: 221        Blocks: 8          IO Block: 4096   regular file
Device: nvme0n1p2  Inode: 131102      Links: 1
Access: (0644/-rw-r--r--)  Uid: (    0/    root)   Gid: (    0/    root)
Access: 2026-10-02 04:12:10.000000000 +0000
Modify: 2026-09-15 11:20:00.000000000 +0000
Change: 2026-09-15 11:20:00.000000000 +0000
```

### The "No Space Left on Device" Inode Trap
A disk can report 0% capacity free even if hundreds of gigabytes of disk space remain!
- Filesystems (like ext4) allocate a **fixed number of inodes** during creation (`mkfs.ext4`).
- If an application generates millions of microscopic 1-byte files (e.g., poorly configured session files or mail queues), it exhausts all available inodes.
```bash
# Check inode utilization
df -ih
```
If `IUse%` is **100%**, new files cannot be created.

---

## 3. File Types in Linux

Linux supports seven distinct file types, identifiable by the first character in `ls -l`:

| Symbol | File Type | Description | Creation / Example |
| :--- | :--- | :--- | :--- |
| **`-`** | Regular File | Binary executables, text files, images, archives | `touch file.txt` |
| **`d`** | Directory | A file containing a list of filename-to-inode mappings | `mkdir dir` |
| **`l`** | Symbolic Link | A pointer containing the path to another target file | `ln -s target link` |
| **`c`** | Character Device | Unbuffered sequential streaming hardware interface | `/dev/tty`, `/dev/urandom` |
| **`b`** | Block Device | Buffered, random-access fixed-block storage device | `/dev/sda`, `/dev/nvme0n1` |
| **`s`** | Socket | Unix Domain Socket for inter-process communication | `/run/containerd.sock` |
| **`p`** | Named Pipe (FIFO)| Uni-directional inter-process data channel | `mkfifo my_pipe` |

---

## 4. File Descriptors (FD) & The Open File Table

A **File Descriptor (FD)** is a simple, non-negative integer handle that the Linux kernel assigns to a process whenever it opens or creates a file, pipe, or network socket.

Every Linux process is launched with three default file descriptors pre-attached:

| FD Number | Name | POSIX Constant | Default Target |
| :--- | :--- | :--- | :--- |
| **`0`** | Standard Input (`stdin`) | `STDIN_FILENO` | Keyboard input / pipe ingress |
| **`1`** | Standard Output (`stdout`) | `STDOUT_FILENO` | Terminal display / output pipe |
| **`2`** | Standard Error (`stderr`) | `STDERR_FILENO` | Terminal display (unbuffered) |

```text
PROCESS ADDRESS SPACE (PID 2400)
┌───────────────────────────────────────────────┐
│ File Descriptor Table:                        │
│   FD 0 ──► Pointer to stdin (/dev/pts/1)     │
│   FD 1 ──► Pointer to stdout (/dev/pts/1)    │
│   FD 2 ──► Pointer to stderr (/dev/pts/1)    │
│   FD 3 ──► Pointer to open socket (Port 8080) │
│   FD 4 ──► Pointer to /var/log/app.log       │
└──────────────────────┬────────────────────────┘
                       │
                       ▼
KERNEL SPACE: Open File Table
  [ Open File Entry ] ──► Stores file offset (e.g., byte 4096) and status flags
       │
       ▼
KERNEL SPACE: Inode Table
  [ Inode # 240185 ] ──► Points to physical disk blocks
```

---

## 5. Inspecting File Descriptors via `/proc`

The kernel exposes every running process's active file descriptors through the `/proc/[PID]/fd/` directory:

```bash
# Find PID of Nginx
PID=$(pgrep -f "nginx: master" | head -1)

# List all open file descriptors for this process
ls -l /proc/$PID/fd/
```

Sample Output:
```text
lrwx------ 1 root root 64 Oct  2 07:30 0 -> /dev/null
lrwx------ 1 root root 64 Oct  2 07:30 1 -> /dev/null
l-wx------ 1 root root 64 Oct  2 07:30 2 -> /var/log/nginx/error.log
lrwx------ 1 root root 64 Oct  2 07:30 6 -> socket:[28914]
lr-x------ 1 root root 64 Oct  2 07:30 7 -> /etc/nginx/nginx.conf
```

---

## 6. System Limits: "Too Many Open Files" Error

In high-concurrency environments (Kubernetes pods, API gateways, load balancers), processes frequently crash with:
`java.io.IOException: Too many open files` or `socket: too many open files`.

### The Three Tiers of File Descriptor Limits:

1. **System-Wide Limit:** Maximum total open file descriptors across all processes on the operating system:
   ```bash
   cat /proc/sys/fs/file-max
   # Tune via sysctl:
   sudo sysctl -w fs.file-max=2097152
   ```

2. **Per-User / Process Limits (`limits.conf`):**
   - **Soft Limit:** The current operational limit enforced for the process (can be raised by the user up to the hard limit).
   - **Hard Limit:** The ceiling enforced by root.
   ```bash
   # View current soft limit
   ulimit -Sn
   
   # View current hard limit
   ulimit -Hn
   ```

3. **systemd Service Limits (`LimitNOFILE`):**
   Modern systemd daemons **ignore `/etc/security/limits.conf`**! You must configure file limits directly in their service definition:
   ```ini
   # /etc/systemd/system/my-service.service.d/override.conf
   [Service]
   LimitNOFILE=65536
   ```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [05 - OS Memory Management Paging Swap and mmap](./05-OS-Memory-Management-Paging-Swap-and-mmap.md) | [README](./README.md) | [07 - Signals and Inter Process Communication IPC](./07-Signals-and-Inter-Process-Communication-IPC.md) |
