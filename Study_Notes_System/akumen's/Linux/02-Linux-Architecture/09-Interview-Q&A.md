# 09 - Technical Interview Questions & Answers (Architecture & Internals)

---

### Q1: Describe the 5-layer architecture of a Linux system.
**Answer:**
The Linux operating system is structured into five distinct abstraction layers:
1. **Layer 1: Hardware:** The underlying physical silicon (x86_64/ARM CPU, RAM, MMU, storage disks, NICs).
2. **Layer 2: The Linux Kernel:** The monolithic core running in privileged CPU Ring 0 that manages process scheduling, virtual memory, filesystems, network protocols, and hardware device drivers.
3. **Layer 3: System Libraries:** Standard C runtime implementations (`glibc` or `musl`) providing POSIX API wrappers and buffering over raw system calls.
4. **Layer 4: System Utilities & Shells:** Command interpreters (`bash`), administrative daemons, and GNU core utilities (`ls`, `cp`, `grep`, `systemd`).
5. **Layer 5: User Applications:** User-facing services, web applications, databases, and containerized workloads.

---

### Q2: What is the fundamental difference between User Space and Kernel Space?
**Answer:**
The separation is enforced by hardware CPU privilege rings:
- **Kernel Space (Ring 0):** The execution environment of the Linux kernel. It has unrestricted, direct access to physical memory, CPU control registers, and peripheral hardware. A crash or unhandled null-pointer dereference in Ring 0 results in a fatal **Kernel Panic**.
- **User Space (Ring 3):** The unprivileged execution environment where normal processes and services run. User space processes have isolated virtual memory managed by the MMU and cannot directly execute I/O instructions. If a user space application crashes (e.g., Segmentation Fault), only that individual process terminates; the rest of the operating system remains stable.

---

### Q3: What happens when you type `cp file1.txt file2.txt` and press Enter?
**Answer:**
1. **Shell Interaction:** The shell reads the command from standard input, parses the tokens, resolves `/usr/bin/cp` via `$PATH`, and calls `fork()` to create a child process followed by `execve()` to execute the binary.
2. **Initialization:** The dynamic linker (`ld-linux.so`) maps `libc.so.6` into memory, and the program begins execution.
3. **System Calls:** `cp` issues `openat()` for `file1.txt` (read-only, getting FD 3) and `file2.txt` (write-only/create, getting FD 4).
4. **Data Transfer:** Using `copy_file_range()` (or a `read()`/`write()` loop), the kernel transfers bytes between the files.
5. **Kernel & Storage:** The kernel VFS resolves directory inodes, allocates RAM pages in the Page Cache for the new file, flags them as "dirty", and queues block I/O requests via Direct Memory Access (DMA) to the storage controller.
6. **Completion:** `cp` calls `close()` on both file descriptors and calls `exit_group(0)`. The parent shell wakes up from `wait()` and displays the prompt.

---

### Q4: How does a CPU transition from User Mode to Kernel Mode during a system call?
**Answer:**
1. The user application (via glibc) loads the system call number into register `%rax` and function arguments into registers `%rdi`, `%rsi`, `%rdx`, `%r10`, `%r8`, and `%r9`.
2. The CPU executes the assembly instruction **`syscall`** (on x86_64).
3. The hardware CPU automatically saves the user instruction pointer (`%rip`) and processor flags, switches the CPU privilege level from Ring 3 to Ring 0, and swaps from the user stack to the process's dedicated kernel stack.
4. The CPU jumps to the kernel's centralized system call handler, which looks up the function pointer in `sys_call_table`.
5. Upon completion, the kernel executes **`sysret`**, switching privilege back to Ring 3 and resuming user space execution.

---

### Q5: What is the architectural difference between a Monolithic Kernel and a Microkernel? Where does Linux fit?
**Answer:**
- **Monolithic Kernel:** Core operating system services (scheduler, virtual filesystem, memory management, network stack, device drivers) all execute together inside a single privileged address space (Ring 0). It offers maximum raw performance because subsystems interact via direct C function calls.
- **Microkernel:** Only the absolute bare essentials (basic scheduling, low-level memory, and IPC) run in Ring 0. Drivers and filesystems run as isolated user space server processes. It offers higher fault tolerance but suffers performance penalties from constant IPC context switching.
- **Linux's Classification:** Linux is a **modular monolithic kernel**. It runs everything in Ring 0 for peak performance, but provides runtime modularity through **Loadable Kernel Modules (LKMs)** that can be inserted or removed without rebooting.

---

### Q6: Differentiate between a CPU Mode Switch and a Process Context Switch.
**Answer:**
- **Mode Switch:** A transition between CPU privilege rings (Ring 3 ➔ Ring 0) when a process executes a system call. The calling process remains on the CPU, and its address space is unchanged. It is extremely fast (~50-100 ns).
- **Process Context Switch:** The CPU scheduler suspends one process and begins running a completely different process. It requires saving all CPU registers, switching memory page tables (`CR3` register), and invalidating the CPU Translation Lookaside Buffer (TLB). It is substantially more computationally expensive.

---

### Q7: Why can a process in state `D` (Uninterruptible Sleep) not be killed by `kill -9`?
**Answer:**
State `D` represents a process sleeping inside a kernel device driver routine, waiting for a hardware I/O operation (such as reading a physical disk sector or responding to an NFS RPC call).
To prevent race conditions, data corruption, or leaving kernel drivers in an inconsistent state, the Linux kernel explicitly masks and blocks signal delivery while a thread is in state `D`. The `SIGKILL` signal will remain pending and will not take effect until the underlying I/O operation finishes or times out.

---

### Q8: Why does `free -m` show almost all RAM as "used", and what is the Page Cache?
**Answer:**
Unused RAM is wasted RAM. The Linux kernel uses spare physical memory as the **Page Cache** to keep recently read and written disk blocks cached in RAM. If an application needs those files again, reads are served at RAM speeds rather than disk speeds.
If applications suddenly require more memory, the kernel instantly evicts clean pages from the Page Cache to satisfy the request. The true available memory is indicated in the **`available`** column of `free -m`, not the `free` column.

---

### Q9: What happens when a container in Kubernetes exceeds its memory limit?
**Answer:**
Kubernetes configures memory constraints using the kernel's **cgroups** subsystem (`memory.max`). When the containerized processes allocate memory beyond this threshold, the kernel invokes the **Out-Of-Memory (OOM) Killer**.
The OOM killer evaluates the `oom_score` of processes in that cgroup and terminates the highest offender using **`SIGKILL` (signal 9)**. Kubernetes detects this termination, sets the pod's termination reason to `OOMKilled`, records Exit Code `137` (`128 + 9`), and initiates container restart according to its restart policy.

---

### Q10: What is eBPF, and why is it considered a revolutionary kernel technology?
**Answer:**
eBPF (Extended Berkeley Packet Filter) allows developers to run custom, sandboxed bytecode directly inside the Linux kernel without modifying kernel source code or loading external kernel modules (`.ko`).
The kernel's built-in **eBPF Verifier** statically analyzes the program before execution to guarantee it cannot crash the system, access unauthorized memory, or cause infinite loops. Modern cloud-native tools (like Cilium for Kubernetes networking and Falco for security) use eBPF to achieve near-zero-overhead packet routing, load balancing, and runtime observability directly at the kernel layer.
---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [08 - Troubleshooting](./08-Troubleshooting.md) | [README](./README.md) | [10 - Hands On Practice](./10-Hands-On-Practice.md) |
