# 16 - Multiple Choice Questions (MCQ): Core Components

Test your knowledge on Linux system architecture and components.

---

### Q1. In which CPU protection ring does the Linux Kernel operate?
- A) Ring 3 (Unprivileged Mode)
- B) Ring 1 (User Space Mode)
- C) Ring 0 (Privileged Mode)
- D) Ring 2 (Device Mode)

<details>
<summary><b>View Answer & Explanation</b></summary>

**Correct Answer:** **C) Ring 0 (Privileged Mode)**

**Explanation:**
The Linux Kernel operates in hardware Ring 0 (Kernel Space), granting it unrestricted access to physical hardware, memory management, and system registers.
</details>

---

### Q2. Why is `cd` a shell built-in command rather than an external binary file?
- A) Because `cd` runs faster in RAM.
- B) Because a child process created by an external binary cannot modify the parent shell's working directory.
- C) Because `cd` is part of standard C `glibc`.
- D) Because `cd` requires root permissions.

<details>
<summary><b>View Answer & Explanation</b></summary>

**Correct Answer:** **B) Because a child process created by an external binary cannot modify the parent shell's working directory.**

**Explanation:**
In Linux, process execution isolation prevents a child process from altering its parent's current working directory. `cd` must execute inside the shell process itself.
</details>

---

### Q3. Which command lists shared library dependencies of an executable binary?
- A) `lsmod`
- B) `ldd`
- C) `strace`
- D) `modprobe`

<details>
<summary><b>View Answer & Explanation</b></summary>

**Correct Answer:** **B) `ldd`**

**Explanation:**
`ldd` (List Dynamic Dependencies) prints the shared objects (`.so`) required by a dynamically linked binary executable.
</details>

---

### Q4. What is `glibc`?
- A) A hardware driver for graphics cards.
- B) The GNU C Library that provides standard wrappers over Linux kernel system calls.
- C) A shell scripting engine replacing Bash.
- D) A systemd service supervisor daemon.

<details>
<summary><b>View Answer & Explanation</b></summary>

**Correct Answer:** **B) The GNU C Library that provides standard wrappers over Linux kernel system calls.**

**Explanation:**
`glibc` is the C standard library in Linux systems providing standard system call wrappers (`open`, `read`, `printf`, `malloc`).
</details>

---

## ⬅️ Navigation
- Previous: [15 - Hands-On Practice](file:///c:/Users/binil/OneDrive/Desktop/cloud-devops-engineering-roadmap/Study_Notes_System/akumen's/Linux/07-Core-Components-of-a-Linux-Machine/15-Hands-On-Practice.md)
- Next: [17 - Quick Revision Cheat Sheet](file:///c:/Users/binil/OneDrive/Desktop/cloud-devops-engineering-roadmap/Study_Notes_System/akumen's/Linux/07-Core-Components-of-a-Linux-Machine/17-Quick-Revision.md)
