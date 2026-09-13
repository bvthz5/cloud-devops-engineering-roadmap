# 11 - Multiple Choice Questions (Self-Assessment)

Test your knowledge of the Linux architecture, system calls, kernel subsystems, and the `cp` workflow.

---

### Q1: In modern x86_64 architecture, which CPU privilege ring does the Linux Kernel run in?
- **A)** Ring 1
- **B)** Ring 2
- **C)** Ring 3
- **D)** Ring 0

<details>
<summary>👉 View Answer & Explanation</summary>

**Correct Answer: D**  
**Explanation:** The Linux kernel executes in CPU Ring 0 (Supervisor Mode), granting it unrestricted access to physical memory, hardware ports, and control registers. User space applications execute in Ring 3 (User Mode). Rings 1 and 2 are unused in Linux.
</details>

---

### Q2: What is the primary role of Layer 3 (System Libraries like glibc) in the Linux architectural model?
- **A)** Communicating directly with the physical hardware through PCI buses.
- **B)** Providing high-level POSIX C API wrappers and user space buffering over raw system calls.
- **C)** Managing process scheduling and allocating physical memory pages.
- **D)** Supervising background daemons as PID 1.

<details>
<summary>👉 View Answer & Explanation</summary>

**Correct Answer: B**  
**Explanation:** Layer 3 (`glibc`/`musl`) shields developers from writing raw CPU assembly traps by providing standardized C function wrappers (`open`, `read`, `write`, `printf`), buffering I/O, and managing userland memory allocation (`malloc`).
</details>

---

### Q3: When a program executes a system call on an x86_64 Linux machine, which register holds the system call number?
- **A)** `%rdi`
- **B)** `%rsi`
- **C)** `%rax`
- **D)** `%rbx`

<details>
<summary>👉 View Answer & Explanation</summary>

**Correct Answer: C**  
**Explanation:** Under the x86_64 Linux calling convention, the system call number is loaded into `%rax` (e.g., `0` for read, `1` for write). Arguments 1 through 6 are passed in `%rdi`, `%rsi`, `%rdx`, `%r10`, `%r8`, and `%r9`.
</details>

---

### Q4: Which Linux kernel architecture style best describes the Linux OS?
- **A)** Pure Microkernel
- **B)** Modular Monolithic Kernel
- **C)** Hybrid Exokernel
- **D)** Monolithic Kernel with no dynamic loading capabilities

<details>
<summary>👉 View Answer & Explanation</summary>

**Correct Answer: B**  
**Explanation:** Linux is a monolithic kernel because all its subsystems (VFS, scheduler, network, drivers) execute together in Ring 0 for peak performance. However, it is modular because it supports dynamic Loadable Kernel Modules (LKMs) loaded at runtime via `modprobe`.
</details>

---

### Q5: In the `cp source.txt destination.txt` workflow, what is the purpose of the `copy_file_range` system call on modern Linux kernels?
- **A)** It prints a progress bar on the terminal.
- **B)** It verifies file checksums using hardware hashing.
- **C)** It copies data directly within the kernel without bouncing bytes through user space memory buffers.
- **D)** It compresses the file using gzip before writing to disk.

<details>
<summary>👉 View Answer & Explanation</summary>

**Correct Answer: C**  
**Explanation:** `copy_file_range` allows the kernel to perform in-kernel server-side copying (or block reflink pointers on filesystems like XFS/Btrfs) without copying data to a user space memory buffer and back, eliminating unnecessary mode switches.
</details>

---

### Q6: Why can a process in state `D` (Uninterruptible Sleep) NOT be terminated even by `kill -9`?
- **A)** The process runs with negative nice priority.
- **B)** The process is currently executing in Ring 3 with elevated sudo permissions.
- **C)** The process is sleeping inside a kernel driver waiting for hardware I/O, and the kernel blocks signal delivery to prevent data corruption.
- **D)** The process has registered a custom signal handler for SIGKILL.

<details>
<summary>👉 View Answer & Explanation</summary>

**Correct Answer: C**  
**Explanation:** State `D` represents a thread sleeping inside kernel driver code waiting for disk or network I/O. The kernel disables signal delivery while inside critical driver routines to avoid corrupting hardware or filesystem state. Signals (including SIGKILL) remain pending until I/O completes.
</details>

---

### Q7: What diagnostic command shows the dynamic shared library (.so) dependencies of a compiled binary?
- **A)** `strace`
- **B)** `ldd`
- **C)** `lsmod`
- **D)** `objdump -d`

<details>
<summary>👉 View Answer & Explanation</summary>

**Correct Answer: B**  
**Explanation:** The `ldd` (List Dynamic Dependencies) utility prints the shared libraries required by a program and resolves where they are loaded on the system.
</details>

---

### Q8: What mechanism does the Linux kernel use to speed up disk read and write operations by caching blocks in unused RAM?
- **A)** Swappiness
- **B)** Page Cache
- **C)** Translation Lookaside Buffer (TLB)
- **D)** Memory Ballooning

<details>
<summary>👉 View Answer & Explanation</summary>

**Correct Answer: B**  
**Explanation:** The Linux kernel maintains the Page Cache in spare physical RAM to store copies of files recently read from or written to disk. The kernel dynamically evicts page cache data when applications need memory.
</details>

---

### Q9: Which Linux technology allows running sandboxed, high-performance programs directly inside the kernel at Layer 2 without loading custom `.ko` modules?
- **A)** systemd
- **B)** cgroups
- **C)** eBPF
- **D)** glibc

<details>
<summary>👉 View Answer & Explanation</summary>

**Correct Answer: C**  
**Explanation:** eBPF (Extended Berkeley Packet Filter) allows developers to write safe, sandboxed bytecode that runs directly in the kernel for observability, security, and networking without the risk of causing a kernel panic.
</details>

---

### Q10: When a Kubernetes container is terminated by the Linux OOM Killer, what exit code is reported?
- **A)** 0
- **B)** 1
- **C)** 137
- **D)** 255

<details>
<summary>👉 View Answer & Explanation</summary>

**Correct Answer: C**  
**Explanation:** Exit code 137 indicates termination by `SIGKILL` (signal 9). Standard Unix exit codes for signal termination are calculated as `128 + Signal Number` (`128 + 9 = 137`).
</details>
