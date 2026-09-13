# 14 - Technical Interview Questions & Answers (Linux Kernel Internals)

---

### Q1: What is the Linux Kernel, and what are its four primary responsibilities?
**Answer:**
The Linux Kernel is the core component of the operating system that boots first into memory and acts as an authoritative resource bridge between software applications and physical hardware. Its four core responsibilities are:
1. **Process Management:** Scheduling tasks fairly across CPU cores via CFS/EEVDF, handling context switching, process lifecycles, and container isolation (cgroups and namespaces).
2. **Memory Management:** Allocating physical RAM pages, providing private virtual address spaces, maintaining the Page Cache, and invoking the OOM Killer when memory is exhausted.
3. **Filesystem Management:** Providing a unified Virtual Filesystem (VFS) abstraction over local and network filesystems, managing inodes/dentries, and scheduling block I/O.
4. **Device Control:** Managing hardware peripherals via device drivers, servicing hardware interrupts (IRQs), and exposing standardized device nodes in `/dev`.

---

### Q2: Why is the Linux Kernel classified as a "Modular Monolithic" kernel?
**Answer:**
- It is **Monolithic** because all core subsystems (scheduler, memory manager, VFS, networking stack, and device drivers) execute together in a single privileged address space (**Ring 0**). This delivers maximum raw performance because subsystems communicate via direct, inline C function calls without Inter-Process Communication (IPC) context switch overhead.
- It is **Modular** because it supports **Loadable Kernel Modules (LKMs)**. Hardware drivers, network protocols, and filesystems can be compiled as dynamic `.ko` object files that are loaded into or removed from the running kernel on demand (`modprobe`), eliminating the need to recompile the kernel when adding new hardware.

---

### Q3: Explain the difference between the Buddy Allocator and the SLAB / SLUB Allocator.
**Answer:**
- **The Buddy Allocator:** Manages large chunks of physical RAM in fixed power-of-two multiples of **4 KB pages** (order 0 = 4 KB, order 1 = 8 KB, ... order 10 = 4 MB). It prevents external fragmentation by splitting and coalescing buddy blocks.
- **The SLAB / SLUB Allocator:** Sits on top of the Buddy Allocator. Because kernel data structures (like `task_struct`, `inode`, `dentry`) are small (100 to a few thousand bytes), allocating a full 4 KB page for each object would cause severe internal fragmentation. The SLUB allocator takes whole pages from the Buddy Allocator and carves them into dedicated caches of fixed-size objects for rapid reuse.

---

### Q4: What is the Virtual Filesystem (VFS), and what are its four primary data structures?
**Answer:**
The VFS is an architectural abstraction layer that allows the Linux kernel to support hundreds of disparate filesystems (ext4, XFS, Btrfs, NFS, procfs) through a standardized POSIX API. It defines four primary C object structures:
1. **`superblock`:** Represents an entire mounted filesystem, containing its block size, status, and filesystem operations table.
2. **`inode`:** Represents a specific file or directory metadata record (size, ownership, permissions, data block pointers).
3. **`dentry` (Directory Entry):** Links a human-readable pathname component to an inode number; aggressively cached in RAM via the `dcache`.
4. **`file`:** Represents an open file instance created when a process calls `openat()`; stores the current read/write cursor offset and access flags.

---

### Q5: How does the Completely Fair Scheduler (CFS) determine which task runs next?
**Answer:**
CFS models an "ideal multi-tasking CPU" where every runnable process receives an equal share of compute time.
1. It tracks the execution time of each task via **`vruntime` (Virtual Runtime)**.
2. If a task has a normal priority (`nice 0`), its `vruntime` advances in lockstep with physical wall-clock time. Higher priority tasks (`nice -20`) have their `vruntime` scale up much more slowly.
3. Runnable tasks are arranged in a self-balancing **Red-Black Tree** ordered strictly by `vruntime`.
4. The scheduler always selects the leftmost node (the task with the smallest `vruntime`) to execute next.

---

### Q6: What is a CPU Soft Lockup, and what triggers the kernel watchdog warning?
**Answer:**
A Soft Lockup occurs when a CPU core executes code inside Kernel Space (Ring 0) and remains stuck in a tight loop or spinlock for longer than the watchdog threshold (default: **20 seconds**) without yielding to the scheduler or servicing timer interrupts.
The kernel detects this using a per-CPU watchdog timer thread. If the stuck core fails to update its watchdog timestamp within 20 seconds, the kernel outputs a stack backtrace to `dmesg` (`watchdog: BUG: soft lockup - CPU#X stuck for 22s!`).

---

### Q7: What is the difference between a Fixed LTS Linux distribution and a Rolling Release distribution?
**Answer:**
- **Fixed / LTS (e.g. Ubuntu 24.04 LTS, RHEL 9):** Major versions are released every few years. The kernel ABI and core software packages are frozen for 5 to 10 years, with only critical security patches and bug fixes backported. It provides stability and predictability for enterprise production workloads.
- **Rolling Release (e.g. Arch Linux, openSUSE Tumbleweed):** Has no fixed version cycles. Upstream kernel updates and software packages are pushed to users as soon as they are compiled. It offers bleeding-edge features and new hardware driver support, but carries higher risk of breaking changes.

---

### Q8: What is eBPF, and why is it superior to traditional out-of-tree Kernel Modules (`.ko`)?
**Answer:**
eBPF allows developers to execute sandboxed, event-driven bytecode directly inside the Linux kernel at runtime without modifying kernel source code or loading dynamic modules.
- **Superiority over LKMs:** If an LKM has a bug or null-pointer dereference, it triggers a catastrophic **Kernel Panic** that crashes the host. In contrast, every eBPF program must pass the in-kernel **eBPF Verifier**, which mathematically proves the program cannot crash the kernel, cannot access unauthorized memory, and is guaranteed to terminate.

---

### Q9: What happens when a system suffers from a "Phantom" Kernel SLAB memory leak?
**Answer:**
In a kernel SLAB leak, physical RAM is consumed by internal kernel data structures (such as un-reclaimed dentries or network socket buffers) rather than user space processes.
- **Diagnostic Signature:** `free -m` reports high memory usage, but summing the RSS of all processes in `ps aux` only accounts for a fraction of the consumed RAM.
- **Resolution:** Inspect `/proc/meminfo` under `Slab:` and run `slabtop -s c` to identify the leaking kernel object cache, then drop caches via `/proc/sys/vm/drop_caches`.

---

### Q10: How does Kernel Page Table Isolation (KPTI) protect against the Meltdown vulnerability?
**Answer:**
Historically, the upper half of every process's page table mapped the entire kernel address space into memory for faster system call performance. The Meltdown hardware vulnerability allowed speculative CPU execution to bypass permission checks and read kernel memory from user space.
KPTI resolves this by maintaining **two separate page tables** for every process:
1. **User Mode Page Table:** Contains only user space memory and minimal trampolines to enter the kernel.
2. **Kernel Mode Page Table:** Contains full kernel mappings.
When a system call occurs, the CPU switches to the kernel page table, shielding kernel memory from speculative side-channel leaks.
