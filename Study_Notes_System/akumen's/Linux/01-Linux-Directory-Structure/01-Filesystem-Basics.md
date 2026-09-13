# 01 - Filesystem Basics & Foundational Concepts

---

## 1. The Linux Filesystem Philosophy: Single Inverted Tree

Unlike Windows, which assigns separate drive letters (`C:\`, `D:\`, `E:\`) to each storage volume or partition, **Linux organizes everything into a single inverted hierarchical tree**.

```
Windows Approach:              Linux Approach:
  C:\ (System Drive)                    / (Root Directory)
  ├── Windows                           ├── bin
  └── Program Files                     ├── etc
                                        ├── home
  D:\ (Data Partition)                  │   └── student
  └── Backups                           ├── var
                                        └── mnt
  E:\ (USB Drive)                           └── usb-drive  <-- Mounted anywhere!
  └── Photos
```

In Linux, there are no drive letters. All physical disks, logical volumes, network file systems (NFS), and RAM-backed filesystems are **mounted** into specific directories (called **mount points**) somewhere along this unified tree starting at `/` (root).

---

## 2. The Golden Philosophy: "Everything is a File"

In Unix and Linux systems, almost every system interaction is handled through the standard file I/O interface (`open()`, `read()`, `write()`, `close()`).

- **Documents & Code:** Regular files storing binary or text data.
- **Directories:** Special files containing lists of filenames and their associated **inode numbers**.
- **Hardware Peripherals (Disks, Terminals):** Exposed as device nodes in `/dev/` (e.g., `/dev/nvme0n1`, `/dev/tty1`).
- **Processes & Memory State:** Exposed as text files in `/proc/` (e.g., `/proc/cpuinfo`, `/proc/meminfo`).
- **Inter-Process Communication:** Unix domain sockets (`.sock`) and Named Pipes (FIFOs).

### File Types in Linux (Represented in `ls -l`)

When you run `ls -l`, the very first character of each line denotes its file type:

```bash
$ ls -l /dev/sda /dev/tty /etc/hosts /tmp/test.sock
brw-rw---- 1 root disk   8,  0 Sep 13 10:00 /dev/sda       # 'b' = Block Device
crw-rw-rw- 1 root tty    5,  0 Sep 13 10:00 /dev/tty       # 'c' = Character Device
-rw-r--r-- 1 root root  384 Sep 13 09:30 /etc/hosts       # '-' = Regular File
srwxr-xr-x 1 user user    0 Sep 13 10:05 /tmp/test.sock   # 's' = Socket
```

| Symbol | File Type | Description | Example |
|:---:|---|---|---|
| `-` | **Regular File** | Text files, binaries, images, archives | `/etc/passwd`, `/bin/bash` |
| `d` | **Directory** | Folder containing list of names & inode mappings | `/home`, `/etc/nginx` |
| `l` | **Symbolic Link** | Shortcut / pointer to another file or path | `/bin -> usr/bin` |
| `c` | **Character Device** | Serial stream of unbuffered characters | `/dev/tty`, `/dev/urandom`, `/dev/null` |
| `b` | **Block Device** | Block-addressable storage (reads/writes in blocks) | `/dev/sda`, `/dev/nvme0n1p1` |
| `s` | **Socket** | Inter-process communication (IPC) endpoint | `/var/run/docker.sock` |
| `p` | **Named Pipe (FIFO)** | Unidirectional data pipeline between processes | Created via `mkfifo mypipe` |

---

## 3. What is an Inode (Index Node)?

In Linux filesystems (such as ext4, XFS), files are **not** identified internally by their names. They are identified by an integer called an **inode number**.

### An Inode Stores:
- File Type (regular, directory, symlink, etc.)
- Permissions (rwx for owner, group, others)
- Ownership (UID and GID)
- File size (in bytes)
- Timestamps:
  - `atime` (Access time)
  - `mtime` (Modification of content time)
  - `ctime` (Change of metadata/inode time)
  - `crtime` / `btime` (Creation/birth time, supported on ext4)
- Number of Hard Links (count of directory entries pointing to this inode)
- Pointers to data blocks on disk where actual contents are stored.

> [!IMPORTANT]
> **What an Inode DOES NOT Store:**
> 1. The **File Name**.
> 2. The **File Path**.
> 
> The filename and path exist purely as an entry inside a **directory file**, mapping a human-readable string to an inode number!

```
Directory File Data Block:
┌─────────────────┬──────────────┐
│  Filename       │ Inode Number │
├─────────────────┼──────────────┤
│  app.log        │ 1048577      │
│  nginx.conf     │ 2097153      │
└─────────────────┴──────────────┘
           │
           ▼
     Inode #1048577:
     ├── Size: 24 MB
     ├── Owner: www-data
     ├── Permissions: -rw-r--r--
     └── Data Block Pointers ───> [Block 8901] [Block 8902] ...
```

---

## 4. Virtual Filesystem (VFS) Layer

How can Linux transparently access an ext4 root partition, an XFS database drive, an NFS network mount, and the `/proc` virtual filesystem without altering user space tools?

Through the **Virtual Filesystem (VFS)**:

```
┌─────────────────────────────────────────────────────────────┐
│               User Applications (cat, grep, ls)              │
└──────────────────────────────┬──────────────────────────────┘
                               │ POSIX System Calls (open, read, write)
                               ▼
┌─────────────────────────────────────────────────────────────┐
│                    Linux VFS Interface                      │
└──────┬──────────────┬──────────────┬─────────────┬──────────┘
       │              │              │             │
       ▼              ▼              ▼             ▼
┌─────────────┐┌─────────────┐┌─────────────┐┌─────────────┐
│ ext4 Driver ││  XFS Driver ││  NFS Driver ││ procfs/sysfs│
└──────┬──────┘└──────┬──────┘└──────┬──────┘└──────┬──────┘
       │              │              │              │
       ▼              ▼              ▼              ▼
┌─────────────┐┌─────────────┐┌─────────────┐┌─────────────┐
│ Local Disk  ││ NVMe Array  ││ Network SAN ││ Kernel RAM  │
└─────────────┘└─────────────┘└─────────────┘└─────────────┘
```

The VFS acts as an abstraction bridge. When you execute `cat /proc/cpuinfo` or `cat /etc/hosts`, `cat` runs the exact same standard read system call—VFS handles whether it pulls bytes from physical disk blocks or generates them dynamically from kernel memory.

---

## 5. Linux Naming Rules & Conventions
1. **Case Sensitivity:** `config.yml`, `Config.yml`, and `CONFIG.YML` are three distinct files.
2. **Path Separator:** Linux strictly uses forward slashes `/`. Backward slashes `\` are escape characters.
3. **Hidden Files:** Any file or directory starting with a dot (`.`) is hidden by default (e.g., `.bashrc`, `.ssh/`, `.git/`). Run `ls -a` to view them.
4. **Valid Characters:** Almost any character is valid (including spaces and punctuation), but by standard convention, use lowercase letters, numbers, hyphens (`-`), and underscores (`_`) to avoid shell escaping issues.
