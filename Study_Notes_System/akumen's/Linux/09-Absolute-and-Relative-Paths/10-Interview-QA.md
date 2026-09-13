# 10 - Interview Q&A: Absolute & Relative Paths

Frequently asked interview questions for Linux Systems Engineers, DevOps Engineers, and SREs.

---

### Q1: What is the fundamental difference between an Absolute Path and a Relative Path in Linux?
**Answer:**
- **Absolute Path:** Specifies a file or directory location starting from the Root directory (`/`). It always begins with `/` and resolves identically regardless of your current working directory (`pwd`).
- **Relative Path:** Specifies a location starting from your Current Working Directory (`pwd`). It does not begin with `/` and its resolved target changes whenever `pwd` changes.

---

### Q2: Why is it critical to use Absolute Paths inside Cron jobs and Systemd service units?
**Answer:**
Cron daemons and Systemd service runners execute processes in non-standard or default root working directories (such as `/` or `/root`). If a script inside cron uses relative paths (`python3 script.py` or `./output.log`), the OS looks for the file inside `/root` or `/`, causing script execution to fail with `No such file or directory`. Absolute paths guarantee deterministic execution.

---

### Q3: What is the difference between `.` and `..` in Linux paths?
**Answer:**
- **`.` (Single Dot):** References the **Current Working Directory**. Used when executing local scripts not in `$PATH` (e.g., `./script.sh`) or specifying destination targets (`cp /file .`).
- **`..` (Double Dot):** References the **Parent Directory** one level above the current directory. Used to traverse up the tree hierarchy (e.g., `cd ..` or `../../var/log`).

---

### Q4: How does the shell expand wildcards (`*`, `?`) when executing a command?
**Answer:**
The shell performs **Glob Expansion** before handing arguments to the target executable. When a user runs `ls *.log`, the shell searches the matching directory, expands `*.log` into the list of filenames (e.g., `a.log b.log`), and executes `execve("/usr/bin/ls", ["ls", "a.log", "b.log"])`.

---

## ⬅️ Navigation
- Previous: [09 - Troubleshooting Path Errors](file:///c:/Users/binil/OneDrive/Desktop/cloud-devops-engineering-roadmap/Study_Notes_System/akumen's/Linux/09-Absolute-and-Relative-Paths/09-Troubleshooting.md)
- Next: [11 - Hands-On Practice & Exercises](file:///c:/Users/binil/OneDrive/Desktop/cloud-devops-engineering-roadmap/Study_Notes_System/akumen's/Linux/09-Absolute-and-Relative-Paths/11-Hands-On-Practice.md)
