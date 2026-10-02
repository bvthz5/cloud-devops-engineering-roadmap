# 17 - Quick Revision Cheat Sheet: Core Components of a Linux Machine

A high-density 5-minute reference sheet for Linux system architecture layers and component interactions.

---

## 🚀 Architectural Layers Cheat Sheet

| Layer | Key Components | Space & Privilege | Primary Diagnostic Command |
| :--- | :--- | :--- | :--- |
| **6. User Apps** | Nginx, Docker, Web Browsers, Python | User Space (Ring 3) | `ps aux`, `systemctl status` |
| **5. Shell** | Bash, Zsh, Sh | User Space (Ring 3) | `type <cmd>`, `echo $SHELL` |
| **4. System Utilities** | `ls`, `cat`, `grep`, `ps`, `top` | User Space (Ring 3) | `which <cmd>`, `type -a` |
| **3. System Libraries** | `glibc` (`libc.so.6`), `libssl`, `libm` | User Space (Ring 3) | `ldd /path/to/binary` |
| **System Call Interface** | `syscall` CPU trap transition | Ring 3 ➔ Ring 0 Bridge | `strace <command>` |
| **2. Linux Kernel** | Process Scheduler, VFS, Drivers | Kernel Space (Ring 0) | `uname -r`, `dmesg`, `lsmod` |
| **1. Hardware** | CPU, RAM, NVMe/SSD, NIC | Physical Layer | `lscpu`, `free -h`, `lspci` |

---

## ⚡ Key Rules to Remember

1. **`cd` vs `ls`:** `cd` alters parent shell state so it MUST be a shell built-in. `ls` is an external binary utility (`/usr/bin/ls`).
2. **Ring 0 vs Ring 3:** Kernel runs in Ring 0 (full hardware access); User Space runs in Ring 3 (restricted, uses `syscall` for hardware ops).
3. **`glibc` Compatibility:** Binaries compiled against newer `glibc` versions fail to run on older `glibc` hosts (`GLIBC_x.xx not found`).
---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [16 - MCQ](./16-MCQ.md) | [README](./README.md) | [18 - Related Topics](./18-Related-Topics.md) |
