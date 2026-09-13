# 16 - Multiple Choice Questions (Self-Assessment)

Test your knowledge of the Linux kernel architecture, core responsibilities, memory allocators, and diagnostic tooling.

---

### Q1: What are the four core responsibilities of the Linux Kernel as defined in operating systems theory?
- **A)** Web hosting, DNS resolution, Firewall rules, User interfaces.
- **B)** Process Management, Memory Management, Filesystem Management, Device Control.
- **C)** Package management, Shell scripting, Compiling, Display rendering.
- **D)** Database indexing, Data caching, Virtualization, Cryptography.

<details>
<summary>👉 View Answer & Explanation</summary>

**Correct Answer: B**  
**Explanation:** The foundational responsibilities of the Linux kernel are Process Management (scheduling, context switching), Memory Management (virtual memory, page allocation), Filesystem Management (VFS, inodes, block I/O), and Device Control (drivers, interrupts, DMA).
</details>

---

### Q2: Why does the Linux Kernel employ the SLUB/SLAB allocator on top of the Buddy Allocator?
- **A)** To encrypt kernel data structures stored on physical disk.
- **B)** To avoid internal fragmentation by managing pre-allocated caches of small, fixed-size objects (like inodes and task_structs).
- **C)** To compress virtual memory pages before swapping them out.
- **D)** To translate logical block addresses into physical NVMe flash sectors.

<details>
<summary>👉 View Answer & Explanation</summary>

**Correct Answer: B**  
**Explanation:** The Buddy Allocator manages physical memory in large multiples of 4 KB pages. Because kernel objects (like dentries, inodes, and task_structs) are only a few hundred bytes, allocating a full 4 KB page for each would waste over 90% of memory. The SLUB allocator carves whole pages into pools of small, fixed-size objects.
</details>

---

### Q3: What data structure does the Linux Completely Fair Scheduler (CFS) use to determine which task to run next on a CPU core?
- **A)** First-In, First-Out (FIFO) queue
- **B)** Self-balancing Red-Black Tree ordered by `vruntime`
- **C)** Hash table indexed by Process ID
- **D)** Doubly-linked circular list ordered by PID

<details>
<summary>👉 View Answer & Explanation</summary>

**Correct Answer: B**  
**Explanation:** CFS stores runnable tasks in a self-balancing Red-Black Tree sorted by virtual runtime (`vruntime`). The scheduler always picks the leftmost node (the task that has consumed the least execution time).
</details>

---

### Q4: In the Virtual Filesystem (VFS), which object structure is responsible for linking a human-readable filename string to an inode number?
- **A)** Superblock
- **B)** Inode
- **C)** Dentry (Directory Entry)
- **D)** File Object

<details>
<summary>👉 View Answer & Explanation</summary>

**Correct Answer: C**  
**Explanation:** A Dentry (Directory Entry) maps a directory path string component to its corresponding inode number. Dentries are cached in RAM via the `dcache` to accelerate pathname resolution.
</details>

---

### Q5: What happens when a CPU core experiences a "Soft Lockup" in Linux?
- **A)** The CPU core hardware overheats and shuts down.
- **B)** The core is executing in Kernel Space (Ring 0) and remains stuck in a loop/spinlock for > 20 seconds without yielding to the scheduler.
- **C)** An application in User Space consumes 100% of memory.
- **D)** A storage drive is disconnected while a write is occurring.

<details>
<summary>👉 View Answer & Explanation</summary>

**Correct Answer: B**  
**Explanation:** A Soft Lockup occurs when a CPU core running kernel code spins in a tight loop or deadlock for longer than `kernel.watchdog_thresh` (default 20 seconds) without scheduling other tasks or handling timer interrupts.
</details>

---

### Q6: What is the primary safety advantage of using eBPF over traditional out-of-tree Kernel Modules (`.ko`)?
- **A)** eBPF runs in user space rather than kernel space.
- **B)** eBPF programs are verified before loading to mathematically guarantee they cannot crash the kernel or access invalid memory.
- **C)** eBPF programs do not require any CPU execution cycles.
- **D)** eBPF bypasses the Linux security model completely.

<details>
<summary>👉 View Answer & Explanation</summary>

**Correct Answer: B**  
**Explanation:** Out-of-tree kernel modules can cause a fatal Kernel Panic if they contain a bug or null pointer dereference. In contrast, the in-kernel eBPF Verifier statically checks bytecode before execution, guaranteeing safety, bounded memory access, and termination.
</details>

---

### Q7: Which command dynamically loads a kernel module into RAM along with all of its dependent modules?
- **A)** `insmod`
- **B)** `lsmod`
- **C)** `modprobe`
- **D)** `depmod`

<details>
<summary>👉 View Answer & Explanation</summary>

**Correct Answer: C**  
**Explanation:** While `insmod` only loads a single raw `.ko` file without resolving prerequisites, `modprobe` automatically analyzes `/lib/modules/$(uname -r)/modules.dep` and loads all required dependencies in order.
</details>

---

### Q8: What does the `z` represent in the Linux kernel binary name `/boot/vmlinuz`?
- **A)** Zero-copy networking support
- **B)** Compressed executable format (e.g. gzip, xz, zstd)
- **C)** Zstandard filesystem formatting
- **D)** Zone-based memory architecture

<details>
<summary>👉 View Answer & Explanation</summary>

**Correct Answer: B**  
**Explanation:** The `z` at the end of `vmlinuz` indicates that the kernel binary is compressed on disk to reduce `/boot` storage space and enable faster loading from disk into RAM during boot.
</details>

---

### Q9: Which directory in Linux exposes live, real-time tunable kernel variables that can be modified on the fly without rebooting?
- **A)** `/etc/kernel/`
- **B)** `/var/run/`
- **C)** `/proc/sys/`
- **D)** `/sys/firmware/`

<details>
<summary>👉 View Answer & Explanation</summary>

**Correct Answer: C**  
**Explanation:** The `/proc/sys/` pseudo-directory provides a direct interface into kernel tunable variables. The `sysctl` utility reads and writes directly to nodes within `/proc/sys/`.
</details>

---

### Q10: How does a "Rolling Release" Linux distribution differ from an "LTS Fixed Release"?
- **A)** Rolling distributions do not use the Linux kernel.
- **B)** Rolling releases push new software and kernel updates continuously as soon as available, without major version milestones.
- **C)** Rolling releases support only ARM64 architecture.
- **D)** Rolling releases freeze package versions for 10 years.

<details>
<summary>👉 View Answer & Explanation</summary>

**Correct Answer: B**  
**Explanation:** Rolling release distributions (like Arch Linux) do not have fixed version numbers (e.g., "22.04"). Instead, packages and upstream kernel versions are updated continuously, delivering bleeding-edge software at the expense of potential stability regressions.
</details>
---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [15 - Hands On Practice](./15-Hands-On-Practice.md) | [README](./README.md) | [17 - Quick Revision](./17-Quick-Revision.md) |
