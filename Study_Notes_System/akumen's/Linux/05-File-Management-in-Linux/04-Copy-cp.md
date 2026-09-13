# 04 - Copying Files & Directories: `cp`

The `cp` command duplicates files and directories, either within the same filesystem or across different storage volumes.

---

## 1. Basic Copy Operations

```bash
# 1. Copy a file to a new name in the same directory:
$ cp config.yml config.yml.bak

# 2. Copy a file into an existing directory:
$ cp app.py /opt/my_app/

# 3. Copy multiple files into a destination directory:
$ cp main.go go.mod go.sum /tmp/build/
```

---

## 2. Critical Flags & Operational Options

| Flag | Name | Function & DevOps Importance |
|:---:|---|---|
| **`-a`** | **Archive** | **The Gold Standard for DevOps:** Preserves all file attributes, permissions, timestamps, ownership, and symbolic links recursively (`-dR --preserve=all`). |
| **`-r` / `-R`**| Recursive | Recursively copies directories and their nested contents. |
| **`-i`** | Interactive | Prompts before overwriting an existing destination file. |
| **`-f`** | Force | Removes and recreates destination files if they cannot be opened. |
| **`-u`** | Update | Copies only if the source file is **newer** than the destination or if the destination file is missing. |
| **`-v`** | Verbose | Displays files as they are copied. |
| **`-p`** | Preserve | Preserves timestamps, ownership (UID/GID), and permissions modes. |
| **`--reflink`**| Copy-on-Write | Creates instantaneous instant clones without duplicating disk blocks on CoW filesystems (XFS, Btrfs, ZFS). |

---

## 3. Why `cp -a` is Mandatory in System Administration

If you use standard `cp -r` to copy a system configuration or web directory:
- The copied files will be assigned **your current user and group** as owner.
- The modification timestamps will be updated to **the exact current second**, wiping out historical deployment dates.
- Symbolic links will be followed and replaced with full copies of their target files!

```bash
# WRONG (loses ownership, timestamps, and dereferences links):
$ sudo cp -r /etc/nginx /etc/nginx.backup

# CORRECT (preserves identical permissions, owners, timestamps, and symlinks):
$ sudo cp -a /etc/nginx /etc/nginx.backup
```

---

## 4. Modern Instant Cloning: `--reflink`

On modern filesystems supporting Copy-on-Write (such as XFS and Btrfs):
```bash
# Instant zero-space copy:
$ cp --reflink=always huge_database.db snapshot.db
```
The kernel creates a new file metadata pointer that points to the exact same physical disk blocks. Disk space is only consumed when one of the files is subsequently modified!
