# 03 - Paths, Navigation & Environment Mechanics

Navigating and referencing files accurately is critical for writing reliable shell scripts, Dockerfiles, and automation playbooks.

---

## 1. Absolute vs. Relative Paths

Every path in Linux tells the shell where to locate a file or directory.

```
Absolute Path: Always starts from root '/'
Example: /home/student/projects/app.py

Relative Path: Starts from current working directory (pwd)
Example: ./projects/app.py   OR   ../student/projects/app.py
```

### Quick Comparison

| Characteristic | Absolute Path | Relative Path |
|---|---|---|
| **Starting Point** | Always starts with `/` | Never starts with `/` |
| **Independence** | Independent of where you currently are in the filesystem | Dependent on the current working directory (`$PWD`) |
| **Best Use Case** | Cron jobs, systemd service units, production automation scripts | Quick interactive CLI commands, modular code referencing relative assets |
| **Example** | `/var/log/nginx/access.log` | `nginx/access.log` (if currently in `/var/log`) |

---

## 2. Essential Path Symbols & Shortcuts

Linux provides standard shorthand symbols recognized across all shells and system utilities:

| Symbol | Meaning | Example Command | What It Does |
|:---:|---|---|---|
| **`/`** | Root directory | `cd /` | Switches to the top-level root directory. |
| **`.`** | Current directory | `cp /tmp/app.py .` | Copies `app.py` into the folder you are currently in. |
| **`..`** | Parent directory | `cd ..` | Moves up one level in the directory hierarchy. |
| **`~`** | Current user's home | `cd ~/logs` | Resolves to `/home/<username>/logs` (or `/root/logs` for root). |
| **`~username`** | Specific user's home | `ls ~student` | Resolves to `/home/student`. |
| **`-`** | Previous working directory | `cd -` | Toggles back to the directory you were previously in. |
---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [02 - Directory Structure](./02-Directory-Structure.md) | [README](./README.md) | [04 - Important Directories Deep Dive](./04-Important-Directories-Deep-Dive.md) |
