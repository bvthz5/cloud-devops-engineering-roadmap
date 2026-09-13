# 05 - File and Storage Management

Filesystems bridge the gap between logical, human-readable data files and physical non-volatile storage media (SSDs, NVMe drives, HDDs).

---

## 1. The File Abstraction

A **file** is a named, persistent logical collection of bytes recorded on secondary storage.

### Common File Attributes:
- **Identifier:** Internal unique integer identifying the file within the filesystem (e.g., Inode number).
- **Name:** Human-readable string.
- **Type:** Differentiates regular files, directories, symlinks, sockets, and character/block devices.
- **Location:** Pointers to physical blocks or extents on disk.
- **Size:** Current size in bytes and allocated block count.
- **Protection:** Access control bits (`rwx` permissions for Owner, Group, Others).
- **Timestamps:** Access time (`atime`), modification time (`mtime`), metadata change time (`ctime`).

---

## 2. File Allocation Methods

How does the operating system map logical file byte offsets (`0` to `N`) to physical disk blocks?

```
1. Contiguous Allocation:
   File A: [Block 10] [Block 11] [Block 12] [Block 13]
   • Fast sequential access, but causes severe External Fragmentation.

2. Linked Allocation:
   File B: [Block 22] ──► [Block 89] ──► [Block 104] ──► [Block 05]
   • No external fragmentation, but Random Access is terrible (must traverse pointers).

3. Indexed Allocation (Unix Inodes / Extents):
   File C: Inode Table
           ├── Direct Blocks   ──► [Block 30], [Block 31], [Block 32]
           ├── Indirect Block  ──► Pointer to Index Block ──► [Blocks ...]
           └── Extents (ext4)  ──► "Start at Block 500 for 128 contiguous blocks"
```

| Method | Advantages | Disadvantages |
|---|---|---|
| **Contiguous** | Ultra-fast sequential reads; ideal for optical discs (CD/DVD). | External fragmentation; impossible to grow file without relocating. |
| **Linked** | Zero external fragmentation; files can grow easily. | Random access requires sequential traversal; pointer corruption destroys remaining file. |
| **Indexed (Inodes)** | Fast random access; dynamic file growth; no external fragmentation. | Index block storage overhead for small files. |

---

## 3. Directory Structures

Operating systems organize files using directory structures:

```
Hierarchical Tree Directory:
                      / (Root)
         ┌────────────┼────────────┐
       /bin         /home        /var
                 ┌────┴────┐       │
               /alice    /bob    /log
```

- **Single-Level Directory:** All files in a single flat namespace (causes filename collisions; used in ancient embedded systems).
- **Hierarchical Tree Directory:** Standard in modern OSes. Allows arbitrary nesting of subdirectories, paths, and isolated user namespaces.
- **Acyclic-Graph Directory:** Allows files and directories to have multiple parent paths via **Hard Links** and **Symbolic (Soft) Links**.

---

## 4. Crash Consistency & Journaling Filesystems

In legacy filesystems (like FAT32 or Linux ext2), a power cut or system crash in the middle of a file write left metadata in an inconsistent state. Rebooting required running **`fsck`** across the entire multi-terabyte disk, taking hours.

Modern filesystems (ext4, XFS, NTFS) use **Journaling (Write-Ahead Logging)**:

```
1. PREPARE:
   Write proposed metadata changes to a small circular log (the Journal on disk).

2. COMMIT:
   Mark journal transaction as "Committed".

3. WRITE:
   Write real data and update filesystem structures (Inodes, Bitmaps).

4. CHECKPOINT:
   Erase journal entry.

* If power dies during Step 3:
  On boot, the kernel replays the committed journal log in 2 seconds!
```
