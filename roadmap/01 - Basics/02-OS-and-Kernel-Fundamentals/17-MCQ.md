# 17 — Multiple Choice Questions (Self-Assessment)

Test your understanding of Operating System and Kernel Fundamentals. Each question contains four options and an expandable answer with a detailed technical explanation.

---

### Q1: What happens when an unprivileged user space program executes the `syscall` assembly instruction?
- **A)** The CPU immediately executes a hardware reboot to reset peripheral controllers.
- **B)** The CPU switches privilege levels from Ring 3 to Ring 0, swaps the stack pointer to the kernel stack, and jumps to the kernel's system call dispatcher routine.
- **C)** The operating system kernel terminates the calling process with a `SIGSEGV` error.
- **D)** The program directly accesses hardware registers without kernel mediation.

<details>
<summary>▶ View Answer & Explanation</summary>

**Correct Answer: B**  
**Explanation:** The `syscall` instruction is a hardware trap that transitions the CPU from User Mode (Ring 3) to Privileged Kernel Mode (Ring 0), swaps to the kernel stack, and branches to the address configured in `MSR_LSTAR` to execute the requested service.
</details>

---

### Q2: Why is Linux classified as a Monolithic Kernel despite supporting Loadable Kernel Modules (LKMs)?
- **A)** Because all device drivers, network stacks, filesystems, and memory managers execute inside the same privileged Ring 0 address space.
- **B)** Because Linux does not support dynamic memory allocation for user space processes.
- **C)** Because the kernel cannot be recompiled once installed on a machine.
- **D)** Because it uses microsecond-level message passing between user space server processes.

<details>
<summary>▶ View Answer & Explanation</summary>

**Correct Answer: A**  
**Explanation:** In a Monolithic kernel, core subsystems (filesystems, network stack, drivers) run together in Ring 0 privileged memory, allowing high-speed direct in-memory communication. LKMs can be loaded dynamically, but once loaded, they execute directly within the privileged kernel address space.
</details>

---

### Q3: What is the primary operational difference between `fork()` and `execve()` in Linux process management?
- **A)** `fork()` loads a new binary from disk; `execve()` creates a copy of the parent process.
- **B)** `fork()` creates a new child process duplicating the calling process using Copy-On-Write; `execve()` replaces the calling process's memory image with an entirely new program binary.
- **C)** `fork()` can only be called by root; `execve()` can be called by any unprivileged user.
- **D)** `fork()` creates a thread; `execve()` creates a container.

<details>
<summary>▶ View Answer & Explanation</summary>

**Correct Answer: B**  
**Explanation:** `fork()` spawns an exact clone of the caller with a new PID and duplicated page tables using Copy-On-Write. `execve()` takes an existing process and replaces its code, heap, and stack with a brand new executable binary from disk, retaining its PID and file descriptors.
</details>

---

### Q4: Why does executing `kill -9 <PID>` fail to eliminate a Zombie process?
- **A)** Because the Zombie process runs with root privileges and ignores signals from non-root users.
- **B)** Because the Zombie process has blocked all signals using `sigprocmask()`.
- **C)** Because the Zombie process has already completed execution and exited; it has no running code or user space memory to receive signals.
- **D)** Because the process is running inside a separate network namespace.

<details>
<summary>▶ View Answer & Explanation</summary>

**Correct Answer: C**  
**Explanation:** A zombie (`defunct`) has already terminated. It consumes no CPU or memory. Its entry remains in the kernel's process table only to preserve its exit code until its parent calls `wait()`. Signals can only be delivered to active processes.
</details>

---

### Q5: What is the purpose of the Copy-On-Write (COW) optimization during process creation?
- **A)** It writes memory pages to disk before execution begins.
- **B)** It allows parent and child processes to share the exact same physical RAM pages read-only, allocating a new page only when one process attempts to write to it.
- **C)** It encrypts memory buffers to protect against buffer overflow exploits.
- **D)** It prevents threads from accessing global variables concurrently.

<details>
<summary>▶ View Answer & Explanation</summary>

**Correct Answer: B**  
**Explanation:** Copy-On-Write makes `fork()` near-instantaneous. Instead of copying megabytes or gigabytes of memory, both processes share the same read-only physical memory frames. When a write occurs, the MMU triggers a minor page fault, and the kernel duplicates only the specific 4 KB page being modified.
</details>

---

### Q6: Which Linux process state indicates that a thread is blocked waiting on an uninterruptible hardware event (such as disk I/O or NFS)?
- **A)** State `R` (`TASK_RUNNING`)
- **B)** State `S` (`TASK_INTERRUPTIBLE`)
- **C)** State `D` (`TASK_UNINTERRUPTIBLE`)
- **D)** State `T` (`TASK_STOPPED`)

<details>
<summary>▶ View Answer & Explanation</summary>

**Correct Answer: C**  
**Explanation:** State `D` (`TASK_UNINTERRUPTIBLE`) means the process is waiting on critical hardware or driver response (usually disk or network filesystem). The kernel does not deliver signals (even `SIGKILL`) to a process in state `D` until the hardware operation completes.
</details>

---

### Q7: What distinguishes a Thread from a Process in Linux?
- **A)** Threads have their own isolated virtual address spaces; processes share address spaces.
- **B)** Threads share their virtual address space, open file descriptors, and signal handlers with other threads in the same process.
- **C)** Threads run in Kernel Space; processes run in User Space.
- **D)** Processes are scheduled by the kernel; threads are scheduled strictly by hardware timers.

<details>
<summary>▶ View Answer & Explanation</summary>

**Correct Answer: B**  
**Explanation:** In Linux, both are `task_struct` instances. A thread is created with the `clone()` syscall using flags like `CLONE_VM` and `CLONE_FILES`, sharing memory, file descriptors, and file system context with its parent process.
</details>

---

### Q8: What is a Major Page Fault?
- **A)** A memory segmentation fault that crashes an application with `SIGSEGV`.
- **B)** A condition where an allocated page is already in physical RAM but missing from the process's page table.
- **C)** A hardware interrupt indicating the requested memory page is not present in RAM and must be read from disk storage or swap.
- **D)** A parity check failure on the physical DDR RAM module.

<details>
<summary>▶ View Answer & Explanation</summary>

**Correct Answer: C**  
**Explanation:** A Major Page Fault occurs when the required page is not in physical RAM (it has been swapped out, or needs to be loaded from a file/binary on disk). The kernel must suspend the process while performing slow disk I/O to bring the page into RAM.
</details>

---

### Q9: Which file descriptor integer corresponds to Standard Error (`stderr`)?
- **A)** `0`
- **B)** `1`
- **C)** `2`
- **D)** `3`

<details>
<summary>▶ View Answer & Explanation</summary>

**Correct Answer: C**  
**Explanation:** By POSIX standard, standard input (`stdin`) is FD `0`, standard output (`stdout`) is FD `1`, and standard error (`stderr`) is FD `2`.
</details>

---

### Q10: Which Linux signal is sent by the operating system (and Kubernetes) to allow a process to perform a graceful shutdown before being forcibly killed?
- **A)** `SIGKILL` (Signal 9)
- **B)** `SIGTERM` (Signal 15)
- **C)** `SIGSTOP` (Signal 19)
- **D)** `SIGSEGV` (Signal 11)

<details>
<summary>▶ View Answer & Explanation</summary>

**Correct Answer: B**  
**Explanation:** `SIGTERM` (15) is the standard termination signal. It can be caught by application code to close database connections, flush buffers, and exit cleanly. `SIGKILL` (9) cannot be caught or handled and terminates the process immediately.
</details>

---

### Q11: What is the primary function of Linux Namespaces?
- **A)** Enforcing CPU bandwidth and memory ceiling limits on processes.
- **B)** Providing process-level isolation of system resources (PIDs, network interfaces, mount points, hostnames).
- **C)** Compiling eBPF programs into native assembly instructions.
- **D)** Encrypting data blocks stored on physical ext4 filesystems.

<details>
<summary>▶ View Answer & Explanation</summary>

**Correct Answer: B**  
**Explanation:** Linux Namespaces isolate global system resources so that a group of processes sees a private, distinct instance of that resource (e.g., private PID tree, network stack, or mount table). This is the foundation of container isolation.
</details>

---

### Q12: Which kernel parameter must be set to `1` on a Linux server to enable packet forwarding across network interfaces (mandatory for Kubernetes CNI and Docker)?
- **A)** `net.core.somaxconn`
- **B)** `vm.swappiness`
- **C)** `net.ipv4.ip_forward`
- **D)** `fs.file-max`

<details>
<summary>▶ View Answer & Explanation</summary>

**Correct Answer: C**  
**Explanation:** Setting `net.ipv4.ip_forward = 1` instructs the Linux kernel to route incoming packets destined for other IP addresses across its network interfaces, which is essential for container bridge networking and Kubernetes pod-to-pod routing.
</details>
