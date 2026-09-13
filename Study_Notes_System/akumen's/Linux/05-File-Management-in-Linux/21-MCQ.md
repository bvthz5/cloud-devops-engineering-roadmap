# 21 - Multiple Choice Questions (MCQ): File Management in Linux

Test your knowledge on Linux file management commands, flags, redirection, links, permissions, search, and archiving.

---

### Q1. Which command and flag combination allows recursively copying a directory while preserving file attributes like timestamps and ownership?
- A) `cp -r`
- B) `cp -a`
- C) `cp -u`
- D) `cp -f`

<details>
<summary><b>View Answer & Explanation</b></summary>

**Correct Answer:** **B) `cp -a`**

**Explanation:**
- `cp -a` (archive mode) is equivalent to `cp -dR --preserve=all`. It copies recursively while preserving symbolic links, file attributes, timestamps, ownership, and permissions.
- `cp -r` copies recursively but creates new files owned by the current user without preserving original metadata.
</details>

---

### Q2. What happens when you run `cat file1.txt > file2.txt` if `file2.txt` already exists?
- A) Content of `file1.txt` is appended to `file2.txt`.
- B) `file2.txt` is overwritten completely with the content of `file1.txt`.
- C) An error is thrown stating `file2.txt` exists.
- D) Both files are merged.

<details>
<summary><b>View Answer & Explanation</b></summary>

**Correct Answer:** **B) `file2.txt` is overwritten completely with the content of `file1.txt`.**

**Explanation:**
The single `>` output redirection operator truncates (overwrites) the target file if it already exists. To append instead of overwriting, use `>>`.
</details>

---

### Q3. Which command is used to display live log changes in real time as new entries are appended?
- A) `cat -f app.log`
- B) `head -n 20 app.log`
- C) `tail -f app.log`
- D) `less -r app.log`

<details>
<summary><b>View Answer & Explanation</b></summary>

**Correct Answer:** **C) `tail -f app.log`**

**Explanation:**
The `-f` (follow) flag in `tail` keeps the file open and streams incoming text in real time. This is standard practice in DevOps for monitoring live service logs.
</details>

---

### Q4. What is the key difference between a hard link and a symbolic (soft) link in Linux?
- A) Soft links share the same inode number as the target file.
- B) Hard links can cross filesystem boundaries, while soft links cannot.
- C) Deleting the original file breaks a symbolic link, but hard links retain the file data as long as at least one link exists.
- D) Hard links can link directories without root privileges.

<details>
<summary><b>View Answer & Explanation</b></summary>

**Correct Answer:** **C) Deleting the original file breaks a symbolic link, but hard links retain the file data as long as at least one link exists.**

**Explanation:**
- A hard link points directly to the underlying inode. The inode (and underlying data blocks) is deleted only when the link count reaches 0.
- A soft link (`ln -s`) contains the text path of the target file. If the target is deleted, the soft link becomes broken ("dangling").
</details>

---

### Q5. What does the command `chmod 755 script.sh` set the file permissions to?
- A) `rwxr-xr-x`
- B) `rwx------`
- C) `rw-r--r--`
- D) `rwxrwxrwx`

<details>
<summary><b>View Answer & Explanation</b></summary>

**Correct Answer:** **A) `rwxr-xr-x`**

**Explanation:**
- `7` (User) = Read (4) + Write (2) + Execute (1) = `rwx`
- `5` (Group) = Read (4) + Execute (1) = `r-x`
- `5` (Others) = Read (4) + Execute (1) = `r-x`
</details>

---

### Q6. Which command extracts a gzipped tarball (`archive.tar.gz`) into the current directory?
- A) `tar -cvf archive.tar.gz`
- B) `tar -xvf archive.tar.gz`
- C) `tar -xzf archive.tar.gz`
- D) `zip -unzip archive.tar.gz`

<details>
<summary><b>View Answer & Explanation</b></summary>

**Correct Answer:** **C) `tar -xzf archive.tar.gz`**

**Explanation:**
- `-x`: Extract
- `-z`: Filter through `gzip`
- `-f`: File operand
Together `tar -xzf` extracts gzipped tarballs.
</details>

---

### Q7. How do you redirect standard error (stderr) to standard output (stdout) in Bash?
- A) `2>&1`
- B) `1>&2`
- C) `>&2`
- D) `2>stdout`

<details>
<summary><b>View Answer & Explanation</b></summary>

**Correct Answer:** **A) `2>&1`**

**Explanation:**
File descriptor 2 is stderr, and file descriptor 1 is stdout. `2>&1` instructs the shell to send stderr output to wherever stdout is currently directed.
</details>

---

### Q8. Which command removes empty directories only?
- A) `rm -rf`
- B) `rmdir`
- C) `mkdir -d`
- D) `unlink`

<details>
<summary><b>View Answer & Explanation</b></summary>

**Correct Answer:** **B) `rmdir`**

**Explanation:**
`rmdir` deletes directories only if they are completely empty. If a directory contains files or subdirectories, `rmdir` throws an error (`Directory not empty`).
</details>

---

### Q9. In Vim, which key sequence saves changes and exits the editor?
- A) `:q!` followed by Enter
- B) `:wq` followed by Enter
- C) Ctrl+C followed by `:exit`
- D) `:w` followed by `:c`

<details>
<summary><b>View Answer & Explanation</b></summary>

**Correct Answer:** **B) `:wq` followed by Enter**

**Explanation:**
In Vim Command Mode:
- `:w` writes (saves) the file.
- `:q` quits the editor.
Combined as `:wq`, it saves and exits. Alternatively, `ZZ` or `:x` performs the same action.
</details>

---

### Q10. Which command searches for files modified in the last 24 hours under `/var/log`?
- A) `grep -m 1 /var/log`
- B) `find /var/log -mtime -1`
- C) `ls -la /var/log --modified`
- D) `stat -time 24h /var/log`

<details>
<summary><b>View Answer & Explanation</b></summary>

**Correct Answer:** **B) `find /var/log -mtime -1`**

**Explanation:**
`find /var/log -mtime -1` searches for files whose modification time is less than 1 day (24 hours) ago.
</details>
---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [20 - Hands On Practice](./20-Hands-On-Practice.md) | [README](./README.md) | [22 - Quick Revision](./22-Quick-Revision.md) |
