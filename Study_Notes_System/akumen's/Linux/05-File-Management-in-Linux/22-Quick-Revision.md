# 22 - Quick Revision Cheat Sheet: File Management in Linux

A high-density 5-minute reference sheet for Linux file operations, syntax, and options.

---

## 🚀 1. Essential Commands Summary

| Command | Purpose | Essential Flags / Usage |
| :--- | :--- | :--- |
| `pwd` | Print Working Directory | `pwd` |
| `cd` | Change Directory | `cd /path`, `cd ..` (up), `cd ~` (home), `cd -` (previous) |
| `ls` | List directory contents | `ls -la` (all long format), `ls -lh` (human readable), `ls -lt` (time sorted) |
| `mkdir` | Make directory | `mkdir -p dir1/dir2/dir3` (nested parent creation) |
| `rmdir` | Remove empty directory | `rmdir empty_dir` |
| `rm` | Remove files/directories | `rm file.txt`, `rm -rf dir/` (recursive force deletion) |
| `cp` | Copy files/directories | `cp file1 file2`, `cp -r dir1 dir2`, `cp -a dir1 dir2` (preserve mode) |
| `mv` | Move / rename files | `mv old_name new_name`, `mv file.txt /dest/` |
| `cat` | Concatenate & view file | `cat file.txt`, `cat -n file.txt` (line numbers) |
| `tac` | View file in reverse | `tac log.txt` |
| `less` | Page through file | `less file.txt` (Navigate with `j/k`, search `/`, quit `q`) |
| `head` | View top lines | `head -n 20 file.txt` |
| `tail` | View bottom lines | `tail -n 50 file.txt`, `tail -f log.txt` (live follow) |
| `nano` | Simple CLI editor | `nano file.txt` (Save: `Ctrl+O`, Exit: `Ctrl+X`) |
| `vim` | Advanced CLI editor | `vim file.txt` (Insert mode: `i`, Command: `Esc`, Save/Quit: `:wq`) |
| `echo` | Print string / variable | `echo "Hello"`, `echo $PATH` |
| `touch` | Create empty / update timestamp | `touch newfile.txt` |
| `chmod` | Change permissions | `chmod 755 script.sh`, `chmod +x script.sh` |
| `chown` | Change owner/group | `chown user:group file.txt`, `chown -R user:group dir/` |
| `ln` | Create link | `ln target hardlink`, `ln -s target softlink` |
| `find` | Search directory hierarchy | `find /path -name "*.log"`, `find /path -mtime -7` |
| `grep` | Search text patterns | `grep -rnI "ERROR" /var/log/` |
| `tar` | Archive files | `tar -czvf archive.tar.gz dir/` (create), `tar -xzvf archive.tar.gz` (extract) |

---

## 🔀 2. I/O Redirection Reference

| Syntax | Descriptor | Meaning |
| :--- | :--- | :--- |
| `command > file` | `1>` (stdout) | Overwrite standard output to `file` |
| `command >> file` | `1>>` (stdout) | Append standard output to `file` |
| `command 2> file` | `2>` (stderr) | Redirect standard error to `file` |
| `command > file 2>&1` | `1` & `2` | Redirect both stdout and stderr to `file` |
| `command &> file` | `1` & `2` | Bash shortcut for both stdout and stderr redirection |
| `cmd1 \| cmd2` | Pipe | Send stdout of `cmd1` as input to `cmd2` |

---

## 🔒 3. Permission Octal Quick Reference

| Digit | Binary | Permission | Description |
| :---: | :---: | :---: | :--- |
| **7** | `111` | `rwx` | Read, Write, Execute |
| **6** | `110` | `rw-` | Read, Write |
| **5** | `101` | `r-x` | Read, Execute |
| **4** | `100` | `r--` | Read only |
| **0** | `000` | `---` | No permissions |

**Common Permission Sets:**
- `chmod 644 file` -> Owner: `rw-`, Group: `r--`, Others: `r--` (Standard file)
- `chmod 755 script` -> Owner: `rwx`, Group: `r-x`, Others: `r-x` (Executable script/directory)
- `chmod 600 key.pem` -> Owner: `rw-`, Group: `---`, Others: `---` (Private SSH Key)
- `chmod 700 dir` -> Owner: `rwx`, Group: `---`, Others: `---` (Private directory)

---

## ⚠️ 4. Safety Golden Rules

1. **Always test wildcards before deletion:**
   Run `ls *.tmp` before running `rm *.tmp`.
2. **Never run unvalidated destructive commands with sudo:**
   `rm -rf /` or `rm -rf / path/to/dir` (notice the accidental space).
3. **Use `-i` for interactive prompts when overwriting or removing:**
   `cp -i`, `mv -i`, `rm -i`.

---

## ⬅️ Navigation
- Previous: [21 - Multiple Choice Questions](file:///c:/Users/binil/OneDrive/Desktop/cloud-devops-engineering-roadmap/Study_Notes_System/akumen's/Linux/05-File-Management-in-Linux/21-MCQ.md)
- Next: [23 - Related Topics & Next Steps](file:///c:/Users/binil/OneDrive/Desktop/cloud-devops-engineering-roadmap/Study_Notes_System/akumen's/Linux/05-File-Management-in-Linux/23-Related-Topics.md)
