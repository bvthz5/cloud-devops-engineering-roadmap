# 19 - Technical Interview Questions & Answers (File Management)

---

### Q1: What is the technical difference between a Hard Link and a Symbolic (Soft) Link?
**Answer:**
- **Hard Link:** A directory entry that points directly to the target file's existing Inode. It shares the exact same inode number, permissions, ownership, and data blocks. Deleting the original filename does not destroy the data as long as at least one hard link remains. Hard links cannot span across different filesystems or point to directories.
- **Symbolic Link:** A separate, small file with its own unique inode number and file type `l` that contains a text path string pointing to the target file. It can cross filesystem boundaries and link to directories. If the target file is deleted, the symlink becomes dangling/broken.

---

### Q2: Why should a DevOps engineer use `cp -a` instead of `cp -r` when backing up system configuration folders?
**Answer:**
- `cp -r` copies files recursively, but assigns the current user/group as the new owner, resets modification timestamps to the current second, and dereferences symbolic links into full duplicate files.
- `cp -a` (Archive mode) combines `-dR --preserve=all`. It preserves original file permissions, user/group ownership (UID/GID), exact timestamps (`atime`/`mtime`), symbolic links without dereferencing, and extended attributes.

---

### Q3: What is the difference between `tail -f` and `tail -F`? Which is better for monitoring production application logs?
**Answer:**
- **`tail -f`:** Follows the file by tracking its open **File Descriptor**. If `logrotate` rotates the log (renaming `app.log` to `app.log.1` and creating a new `app.log`), `tail -f` remains stuck watching the rotated inactive file.
- **`tail -F`:** Follows the file by tracking its **File Name** (retry mode). If the log is rotated or recreated, `tail -F` automatically detects the new file and resumes streaming. **`tail -F` is mandatory for production log monitoring.**

---

### Q4: What does the Sticky Bit do when applied to a directory, and where is it commonly used?
**Answer:**
When the Sticky Bit (octal `1000`, displayed as `t` in `ls -l`) is set on a directory, it restricts file deletion and renaming. Even if a user has write (`w`) permissions on the directory, they are only allowed to delete or rename files **that they personally own** (or if they are root).
It is most famously used on public temporary directories like **`/tmp`** (`chmod 1777 /tmp`) to prevent users from deleting each other's temporary files.

---

### Q5: How do `cat`, `more`, and `less` differ in memory handling when viewing a 50 GB log file?
**Answer:**
- **`cat`:** Reads the entire file sequentially and dumps all 50 GB to `stdout`, flooding the terminal buffer and freezing the session.
- **`more`:** Loads early parts into a pager, but only supports forward scrolling and does not handle massive files efficiently.
- **`less`:** Lazily loads only the text visible on the current terminal window screen (~40 lines). It opens a 50 GB file instantaneously with minimal RAM overhead and allows full forward and backward searching/scrolling.

---

### Q6: How does octal permission notation work in `chmod 755`?
**Answer:**
Octal permissions are calculated by summing values for Read (`r` = 4), Write (`w` = 2), and Execute (`x` = 1) across three user categories:
- First digit (`7` = 4+2+1): **Owner** has Read, Write, and Execute (`rwx`).
- Second digit (`5` = 4+0+1): **Group** has Read and Execute (`r-x`).
- Third digit (`5` = 4+0+1): **Others** have Read and Execute (`r-x`).

---

### Q7: How do you fix the error `bash: /usr/bin/rm: Argument list too long` when trying to delete 300,000 files?
**Answer:**
The error occurs because shell wildcard expansion (`rm *`) exceeds the kernel's `ARG_MAX` command-line argument buffer limit.
To bypass shell expansion, use `find` to process files in kernel space:
```bash
find . -type f -delete
```

---

### Q8: What is the difference between `>` and `>>`, and how does `2>&1` work in shell redirection?
**Answer:**
- **`>`:** Redirects standard output (`stdout`, FD 1) to a file, **overwriting** existing contents.
- **`>>`:** Redirects standard output (`stdout`, FD 1) to a file, **appending** to existing contents.
- **`2>&1`:** Redirects standard error (`stderr`, FD 2) to the exact same file handle currently used by standard output (`stdout`, FD 1), combining both output streams into a single log file.

---

### Q9: How do you safely zero out an active 40 GB production log file without restarting the application or losing disk space?
**Answer:**
Running `rm` on an active log file unlinks the filename, but the running application process still holds the open file descriptor, meaning disk blocks are not freed and the application will fail to log.
To reclaim space safely without stopping the process, truncate the file in place:
```bash
sudo truncate -s 0 /var/log/app.log
# OR
: > /var/log/app.log
```

---

### Q10: How does `umask` determine the default permissions of a newly created file?
**Answer:**
The kernel calculates default creation permissions by taking base defaults (`666` for files, `777` for directories) and subtracting the `umask` value using a bitwise NOT operation.
With a standard `umask` of `022`:
- **New File:** `666 - 022 = 644` (`rw-r--r--`)
- **New Directory:** `777 - 022 = 755` (`rwxr-xr-x`)
---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [18 - Troubleshooting](./18-Troubleshooting.md) | [README](./README.md) | [20 - Hands On Practice](./20-Hands-On-Practice.md) |
