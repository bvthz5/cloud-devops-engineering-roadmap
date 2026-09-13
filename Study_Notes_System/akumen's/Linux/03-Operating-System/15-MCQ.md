# 15 - Multiple Choice Questions (Self-Assessment)

Test your mastery of operating systems concepts, Linux distributions, memory management, and process lifecycles.

---

### Q1: What is the primary purpose of the Translation Lookaside Buffer (TLB) in computer architecture?
- **A)** Storing dirty filesystem pages before writing them to disk.
- **B)** Caching virtual-to-physical memory page address translations to accelerate memory access.
- **C)** Managing inter-process communication sockets.
- **D)** Buffering incoming network packets to prevent interrupt livelock.

<details>
<summary>👉 View Answer & Explanation</summary>

**Correct Answer: B**  
**Explanation:** The TLB is an on-chip hardware cache within the Memory Management Unit (MMU) that stores recent virtual-to-physical page mappings, avoiding the latency of walking multi-level page tables in DRAM for every instruction.
</details>

---

### Q2: A process has terminated execution via `exit()`, but its parent has not invoked `wait()`. What state is this process in?
- **A)** Orphan
- **B)** Sleeping (`S`)
- **C)** Zombie (`Z`)
- **D)** Blocked (`D`)

<details>
<summary>👉 View Answer & Explanation</summary>

**Correct Answer: C**  
**Explanation:** A process whose execution is complete but whose exit code has not been read by its parent is a **Zombie**. It consumes no memory or CPU, retaining only an entry in the kernel's process table until reaped.
</details>

---

### Q3: Which of the following is NOT one of the four Coffman conditions necessary for a deadlock?
- **A)** Mutual Exclusion
- **B)** Hold and Wait
- **C)** Preemptive Resource Allocation
- **D)** Circular Wait

<details>
<summary>👉 View Answer & Explanation</summary>

**Correct Answer: C**  
**Explanation:** The condition is **No Preemption** (resources cannot be forcibly taken). If preemptive resource allocation is supported, deadlocks cannot occur because the OS can simply revoke resources from holding processes.
</details>

---

### Q4: In Linux, why does the Load Average metric sometimes climb to high numbers when CPU utilization is nearly 0%?
- **A)** Because Load Average also counts processes sleeping in Uninterruptible Disk I/O State (`D`).
- **B)** Because Load Average measures network bandwidth saturation.
- **C)** Because the Linux kernel calculates Load Average on a logarithmic scale.
- **D)** Because RAM paging operations are excluded from CPU usage but multiplied in load.

<details>
<summary>👉 View Answer & Explanation</summary>

**Correct Answer: A**  
**Explanation:** Unlike traditional Unix which counted only runnable tasks on the CPU, Linux includes processes in uninterruptible sleep (state `D`, usually waiting on disk/NFS I/O). A stalled storage controller causes load average to spike even if CPUs are completely idle.
</details>

---

### Q5: In Paging memory management, what is the key difference between a "Page" and a "Frame"?
- **A)** A Page is a block of physical RAM, while a Frame is a block of disk storage.
- **B)** A Page is a fixed-size block of virtual memory, while a Frame is an identical fixed-size block of physical RAM.
- **C)** A Page is managed by user space, while a Frame is managed by the BIOS.
- **D)** A Page stores executable code, while a Frame stores process stack data.

<details>
<summary>👉 View Answer & Explanation</summary>

**Correct Answer: B**  
**Explanation:** Virtual memory is divided into fixed-size contiguous blocks called **Pages** (typically 4 KB). Physical DRAM memory is divided into matching physical blocks called **Frames**.
</details>

---

### Q6: Which Linux distribution family uses `.rpm` packages and the `dnf`/`yum` package manager?
- **A)** Debian Family
- **B)** Alpine Family
- **C)** Red Hat Family (RHEL, Fedora, Rocky, AlmaLinux)
- **D)** Arch Linux

<details>
<summary>👉 View Answer & Explanation</summary>

**Correct Answer: C**  
**Explanation:** Red Hat Enterprise Linux (RHEL), Fedora, CentOS, Rocky Linux, and AlmaLinux belong to the Red Hat family and utilize RPM packages managed by `dnf` or `yum`.
</details>

---

### Q7: Why did Dennis Ritchie invent the C programming language at Bell Labs in 1972?
- **A)** To write web server applications for the ARPANET.
- **B)** To rewrite the UNIX operating system so it could be portable across different hardware platforms.
- **C)** To replace Python for artificial intelligence workloads.
- **D)** To build the first relational database management system.

<details>
<summary>👉 View Answer & Explanation</summary>

**Correct Answer: B**  
**Explanation:** Prior to C, operating systems were written in hardware-specific assembly language. Ritchie created C to rewrite UNIX, making it the first operating system in history that could be compiled and run across diverse computer architectures.
</details>

---

### Q8: What occurs during memory "Thrashing"?
- **A)** The CPU scheduler deadlocks due to circular wait locks.
- **B)** The OS spends more time swapping pages between RAM and disk than executing user instructions.
- **C)** The OOM killer forcefully terminates all system daemons.
- **D)** A process writes past its allocated array boundary, causing a segmentation fault.

<details>
<summary>👉 View Answer & Explanation</summary>

**Correct Answer: B**  
**Explanation:** Thrashing occurs when total active working set memory exceeds physical RAM. The system continuously faults and swaps pages to and from disk, causing system throughput to plunge to near zero.
</details>

---

### Q9: What distinguishes a "Hard Real-Time" OS from a standard general-purpose OS?
- **A)** It uses solid-state drives instead of mechanical hard drives.
- **B)** Missing a scheduling deadline results in catastrophic total system failure.
- **C)** It does not support multi-threading.
- **D)** It can only run on 32-bit microcontrollers.

<details>
<summary>👉 View Answer & Explanation</summary>

**Correct Answer: B**  
**Explanation:** Hard Real-Time Systems (like those controlling aircraft flight surfaces or medical pacemakers) guarantee strict timing determinism. Missing a deadline by even a few microseconds is considered a fatal failure.
</details>

---

### Q10: What is the purpose of the CPU hardware Mode Bit?
- **A)** To toggle CPU clock frequency between high and low power modes.
- **B)** To enforce separation between privileged Kernel Mode (`0`) and unprivileged User Mode (`1`).
- **C)** To switch between 32-bit and 64-bit instruction execution.
- **D)** To enable hyper-threading on individual cores.

<details>
<summary>👉 View Answer & Explanation</summary>

**Correct Answer: B**  
**Explanation:** The Mode Bit in the CPU status register designates whether the processor is executing in Kernel Mode (Ring 0, full privilege) or User Mode (Ring 3, restricted privilege).
</details>
