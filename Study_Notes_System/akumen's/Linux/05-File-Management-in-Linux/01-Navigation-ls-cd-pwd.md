# 01 - Navigation Essentials: `pwd`, `cd`, and `ls`

Efficient navigation in the Linux command line is the bedrock of system administration and automation scripting.

---

## 1. `pwd` — Print Working Directory

The `pwd` command outputs the full absolute pathname of your current directory.

```bash
$ pwd
/var/log/nginx
```

### Physical vs. Logical Paths:
When you navigate into a symbolic link directory, `pwd` offers two behaviors:
- **`pwd` or `pwd -L` (Logical):** Prints the logical path following symlinks.
- **`pwd -P` (Physical):** Resolves all symbolic links to print the true underlying physical filesystem path.

```bash
$ cd /bin
$ pwd
/bin
$ pwd -P
/usr/bin    # (Reveals that /bin is physically inside /usr)
```

---

## 2. `cd` — Change Directory

The `cd` utility shifts the shell's current working directory.

### Essential Navigation Shortcuts:

| Command | Action / Destination |
|---|---|
| **`cd`** or **`cd ~`** | Jump directly to your personal home directory (`$HOME`). |
| **`cd ..`** | Move up one level to the immediate parent directory. |
| **`cd ../..`** | Move up two levels in the directory tree. |
| **`cd -`** | Toggle back to the previous working directory (`$OLDPWD`). |
| **`cd /`** | Jump to the root directory of the entire filesystem. |
| **`cd ~username`** | Jump directly to another user's home directory (e.g. `cd ~ubuntu`). |

```bash
# Example of toggling between directories using 'cd -':
$ cd /etc/nginx/conf.d/
$ cd /var/log/nginx/
$ cd -
/etc/nginx/conf.d    # (Instantly returned back to conf.d!)
$ cd -
/var/log/nginx       # (Toggled back to logs!)
```

---

## 3. `ls` — List Directory Contents

The `ls` command displays files and directories within the specified target path.

### High-Frequency Options:

| Flag | Meaning | Description |
|:---:|---|---|
| **`-l`** | Long format | Displays permissions, hard links, owner, group, size, and timestamp. |
| **`-a`** | All files | Shows hidden files and dotfiles (names starting with `.`). |
| **`-A`** | Almost all | Shows hidden files, but omits the `.` (current) and `..` (parent) entries. |
| **`-h`** | Human-readable | Displays file sizes in KB, MB, GB rather than raw bytes (use with `-l`). |
| **`-t`** | Sort by Time | Sorts files by modification time, newest first. |
| **`-S`** | Sort by Size | Sorts files by size, largest first. |
| **`-r`** | Reverse sort | Reverses any sorting order (e.g., `-tr` shows oldest first, newest at the bottom). |
| **`-R`** | Recursive | Recursively lists all subdirectories and their contents. |
| **`-d`** | Directory itself | Lists the directory entry itself rather than its contents. |
| **`-i`** | Inode | Prints the unique filesystem inode number beside each file. |

### Essential Power Combinations:
```bash
# 1. Standard inspection (detailed, human sizes, hidden files):
$ ls -lah

# 2. View newest files at the bottom of the screen (perfect for checking fresh logs):
$ ls -latr /var/log

# 3. Find the biggest files in a folder:
$ ls -lSh /var/log

# 4. Inspect directory permissions themselves (without listing contents):
$ ls -ld /var/log/nginx
drwxr-xr-x 2 www-data adm 4096 Sep 13 10:00 /var/log/nginx
```
