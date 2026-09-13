# 14 - Interview Q&A: Core Components of a Linux Machine

Frequently asked technical interview questions for SysAdmins, DevOps Engineers, and SREs.

---

### Q1: What are the main components of a Linux operating system?
**Answer:**
A Linux machine consists of 6 primary layers:
1. **Hardware Layer:** Physical CPU, RAM, disk storage, and NICs.
2. **Linux Kernel:** Core OS operating in privileged Ring 0 (Process Scheduling, VFS, Memory Management).
3. **Device Drivers:** Software drivers interfacing kernel abstractions with physical hardware controllers.
4. **System Libraries (`glibc`):** Shared wrapper libraries implementing POSIX C APIs and system call traps.
5. **System Utilities:** Command-line programs (`ls`, `cat`, `grep`, `systemctl`).
6. **User Applications & Shell:** Shells (`bash`) and user software (Nginx, Docker, databases).

---

### Q2: Why is `cd` built into the shell, while `ls` is an external binary program?
**Answer:**
`cd` changes the working directory of a process. In Linux, a child process created via `fork()` cannot modify the working directory of its parent process. If `cd` were an external binary (`/usr/bin/cd`), it would execute in a child process, change the child's directory, and exit, leaving the parent shell unchanged. Therefore, `cd` must execute internally within the shell process. `ls` only reads data and outputs text, so it safely executes as an external binary (`/usr/bin/ls`).

---

### Q3: What is `glibc`, and why is it important in Linux?
**Answer:**
`glibc` (GNU C Library) is the foundational C system library on Linux. It translates standardized C library functions (like `printf()`, `malloc()`, `open()`, `read()`) into CPU trap instructions (`syscall`) that transition execution from unprivileged User Space (Ring 3) to privileged Kernel Space (Ring 0).

---

### Q4: Explain the difference between Static and Dynamic Linking.
**Answer:**
- **Static Linking:** All library dependencies are compiled directly into the binary executable file. The resulting binary is self-contained and larger in size, but requires no external `.so` libraries on target machines.
- **Dynamic Linking:** The binary contains reference pointers to shared object libraries (`.so`). At runtime, the dynamic linker (`ld.so`) loads shared libraries into memory. This produces smaller binaries and allows memory sharing across processes, but requires target libraries to exist on the host OS.

---

## ⬅️ Navigation
- Previous: [13 - Troubleshooting Methodology](file:///c:/Users/binil/OneDrive/Desktop/cloud-devops-engineering-roadmap/Study_Notes_System/akumen's/Linux/07-Core-Components-of-a-Linux-Machine/13-Troubleshooting.md)
- Next: [15 - Hands-On Practice & Exercises](file:///c:/Users/binil/OneDrive/Desktop/cloud-devops-engineering-roadmap/Study_Notes_System/akumen's/Linux/07-Core-Components-of-a-Linux-Machine/15-Hands-On-Practice.md)
