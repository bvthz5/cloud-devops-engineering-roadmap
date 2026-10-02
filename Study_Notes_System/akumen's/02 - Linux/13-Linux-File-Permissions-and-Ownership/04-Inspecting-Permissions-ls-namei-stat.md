# 04 - Inspecting Permissions: ls, stat, and namei

Before changing permissions, you must be able to view and understand the current state of files and directories. Linux provides several tools for this, ranging from basic directory listing to deep path resolution.

---

## 1. `ls -l` (Long Listing Format)

This is the most common command used to view permissions and ownership.

```bash
ls -l /etc/passwd
```

**Output Breakdown:**

```text
-rw-r--r-- 1 root root 2795 Aug 10 14:32 /etc/passwd
│          │ │    │    │    │            │
│          │ │    │    │    │            └──▶ File name
│          │ │    │    │    └───────────────▶ Last modified date/time
│          │ │    │    └────────────────────▶ File size in bytes
│          │ │    └─────────────────────────▶ Group Owner
│          │ └──────────────────────────────▶ User Owner
│          └────────────────────────────────▶ Number of hard links
└───────────────────────────────────────────▶ Type & Permissions string
```

### Useful `ls` Flags for Permissions
*   `ls -ld /path/to/dir`: View the permissions of the directory *itself*, rather than its contents. (Crucial for debugging directory access issues).
*   `ls -lh`: Human-readable file sizes (e.g., KB, MB).
*   `ls -la`: Show all files, including hidden files (those starting with `.`, like `.ssh`).

---

## 2. `stat` (File Status)

`stat` provides a more detailed, raw view of a file or directory's metadata, pulling directly from the inode.

```bash
stat /etc/passwd
```

**Example Output:**

```text
  File: /etc/passwd
  Size: 2795            Blocks: 8          IO Block: 4096   regular file
Device: fd01h/64769d    Inode: 131102      Links: 1
Access: (0644/-rw-r--r--)  Uid: (    0/    root)   Gid: (    0/    root)
Access: 2023-08-10 14:32:10.123456789 +0000
Modify: 2023-08-10 14:32:05.987654321 +0000
Change: 2023-08-10 14:32:05.987654321 +0000
 Birth: -
```

**Why use `stat` over `ls -l`?**
*   It clearly shows the **Octal** permission representation (`0644`).
*   It explicitly shows Uid (User ID) and Gid (Group ID) numbers alongside the names.
*   It separates Access Time (atime), Modify Time (mtime - content changed), and Change Time (ctime - metadata/permissions changed).

---

## 3. `namei` (Follow a Pathname)

`namei` is an incredibly powerful, yet often overlooked, troubleshooting tool.

**The Problem:** To read a file like `/var/www/html/index.html`, you need read permissions on the file itself, BUT you also need execute (`x`) permissions on `/`, `/var`, `/www`, and `/html`. If any directory in the chain lacks `x`, you get "Permission denied," even if the file permissions are perfect.

`namei` traces a pathname and lists the permissions for every component in the chain.

```bash
namei -m /var/www/html/index.html
```

**Example Output:**

```text
f: /var/www/html/index.html
 drwxr-xr-x /
 drwxr-xr-x var
 drwxr-xr-x www
 drwx------ html           <-- Problem found! Others cannot enter 'html'
 -rw-r--r-- index.html
```

**Why use `namei -m`?**
*   `-m` displays the modes (permissions).
*   It instantly identifies which directory in a long path is blocking access, saving you from running `ls -ld` manually on every parent folder.

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [03 - Ownership and Groups](./03-Ownership-and-Groups.md) | [README](./README.md) | [05 - chmod Symbolic Notation](./05-chmod-Symbolic-Notation.md) |
