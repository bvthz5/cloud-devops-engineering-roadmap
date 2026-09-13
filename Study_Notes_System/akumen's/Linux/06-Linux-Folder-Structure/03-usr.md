# 03 - The `/usr` Directory (Unix System Resources)

Originally short for "User", modern Linux FHS defines `/usr` as **Unix System Resources** (or *User System Resources*). It contains read-only user data, binaries, documentation, libraries, and header files for system applications.

---

## 📂 Internal Subdirectory Hierarchy of `/usr`

`/usr` mirrors much of the root directory hierarchy and contains second-tier binaries and assets:

```text
/usr/
├── bin/          # Non-essential user command binaries (python, git, curl, gcc)
├── sbin/         # Non-essential system administration binaries
├── lib/          # Libraries for binaries in /usr/bin and /usr/sbin
├── local/        # Custom binaries built from source or installed manually by SysAdmin
├── share/        # Architecture-independent shared data (man pages, themes, docs)
├── include/      # C/C++ header files (.h files for compiling software)
└── src/          # Source code files (e.g., Linux kernel source files)
```

---

## 🔍 Key Subdirectories Explained

### 1. `/usr/bin`
Holds user commands and programs that are not strictly essential for bare-metal single-user boot repair. Examples include `git`, `python3`, `curl`, `jq`, `nginx`, and `docker`.

### 2. `/usr/local/`
The standard location for installing custom software compiled from source (`./configure && make && make install`) or installed manually outside distribution package managers (`apt`/`yum`).
- `/usr/local/bin`
- `/usr/local/etc`
- `/usr/local/lib`
This ensures package manager updates (`apt upgrade`) will never overwrite your manually installed custom tools.

### 3. `/usr/share/`
Contains architecture-independent data:
- `/usr/share/man`: System manual pages read by `man` command.
- `/usr/share/doc`: Package documentation and examples.
- `/usr/share/fonts`: System-wide font files.

---

## ⬅️ Navigation
- Previous: [02 - `/boot` Directory](file:///c:/Users/binil/OneDrive/Desktop/cloud-devops-engineering-roadmap/Study_Notes_System/akumen's/Linux/06-Linux-Folder-Structure/02-boot.md)
- Next: [04 - `/etc` Directory](file:///c:/Users/binil/OneDrive/Desktop/cloud-devops-engineering-roadmap/Study_Notes_System/akumen's/Linux/06-Linux-Folder-Structure/04-etc.md)
