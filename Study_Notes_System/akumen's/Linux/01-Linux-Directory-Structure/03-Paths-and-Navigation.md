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

## 3. Shell Navigation Mastery (`pushd`, `popd`, `dirs`)

While `cd` is fine for basic navigation, experienced engineers use directory stacks for deep multi-directory switching:

```bash
# Push directory onto stack and jump to it
$ pwd
/etc/nginx

$ pushd /var/log/nginx
/var/log/nginx /etc/nginx

# Now do your work in logs...
$ ls -lh

# Pop directory from stack to return immediately
$ popd
/etc/nginx
```

---

## 4. How the Shell Locates Executables: The `$PATH` Variable

When you type a command like `nginx` or `python3`, how does Linux find it without you typing `/usr/bin/nginx`?

### The Search Process:
1. The shell inspects the `$PATH` environment variable.
2. `$PATH` contains a colon-separated list of directories searched in order from left to right:
   ```bash
   $ echo $PATH
   /usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin
   ```
3. The shell checks each directory sequentially for an executable named `nginx`.
4. As soon as a match with execute permissions (`+x`) is found, the shell executes it and stops searching.

```
User types: "nginx"
   │
   ├── Checks /usr/local/sbin/nginx  --> Not found
   ├── Checks /usr/local/bin/nginx   --> Not found
   ├── Checks /usr/sbin/nginx        --> Found! Execute and terminate search.
```

### Why do we need `./script.sh` instead of just `script.sh`?
In Linux, the current directory (`.`) is **intentionally omitted from `$PATH` for security reasons**.
If `.` were in `$PATH`, an attacker could place a malicious executable named `ls` or `sl` in `/tmp`. If an administrator ran `ls` inside `/tmp`, the malicious binary would execute with root privileges!

To run a script in your current directory, you must explicitly specify the relative path:
```bash
$ ./deploy.sh
```

---

## 5. Finding Real Paths & Resolving Symlinks

In modern distributions where `/bin` is a symlink to `/usr/bin`, commands like `realpath` reveal the true underlying physical location:

```bash
$ realpath /bin
/usr/bin

$ which bash
/usr/bin/bash

$ realpath /dev/cdrom
/dev/sr0
```
