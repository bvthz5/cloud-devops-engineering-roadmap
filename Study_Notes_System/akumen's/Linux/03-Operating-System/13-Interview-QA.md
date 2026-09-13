# 13 - Technical Interview Questions & Answers (Operating Systems Theory)

---

### Q1: What is an Operating System, and what are its two foundational perspectives?
**Answer:**
An Operating System is system software that manages physical hardware resources and provides abstract runtime services to applications. It is understood through two core perspectives:
1. **Top-Down (The Extended / Abstract Machine):** It abstracts messy, vendor-specific hardware interfaces (registers, storage sectors, network packets) into clean, standardized abstractions like files, processes, virtual memory, and sockets.
2. **Bottom-Up (The Resource Manager):** It arbitrates competing requests for finite resources (CPU, RAM, storage, network bandwidth) using time-multiplexing (scheduling) and space-multiplexing (memory allocation) to ensure fairness, security, and fault isolation.

---

### Q2: What is the fundamental difference between a Process and a Thread? What resources are shared?
**Answer:**
- **Process:** An independent unit of resource allocation with its own isolated 64-bit virtual address space, file descriptor table, and security credentials. Inter-process communication requires explicit IPC (sockets, pipes, shared memory).
- **Thread:** A lightweight unit of execution within a parent process. Multiple threads share the exact same address space (code, global data, heap) and open file descriptors, but each thread maintains its own:
  - Unique Thread ID (TID)
  - Program Counter (PC)
  - Dedicated Call Stack (local variables)
  - CPU Registers

---

### Q3: Walk through the 5-state process lifecycle. What is a Zombie process and how do you fix it?
**Answer:**
- **Lifecycle:**
  1. **New:** Process is created via `fork()`/`clone()`.
  2. **Ready:** Waiting in the scheduler run-queue to be assigned CPU time.
  3. **Running:** Actively executing instructions on a CPU core.
  4. **Waiting / Blocked:** Sleeping while waiting for an I/O event or signal.
  5. **Terminated:** Execution finished; waiting for exit code to be reaped.
- **Zombie Process:** A process that has called `exit()`, releasing its memory, but whose parent has not yet read its exit status code via `wait()` or `waitpid()`. It consumes 0 bytes of RAM, but occupies a slot in the kernel PID table.
- **How to Fix:** You cannot kill a zombie with `kill -9` because it is already dead. You must send `SIGCHLD` to the parent process to trigger reaping, or kill/restart the negligent parent process, causing the zombie to be adopted and reaped by PID 1 (`systemd`).

---

### Q4: What is Virtual Memory, and how does Paging eliminate External Fragmentation?
**Answer:**
Virtual Memory creates the illusion that each process has a vast, contiguous address space, isolated from other processes.
In contiguous allocation, loading and unloading programs of varying sizes leaves scattered gaps of unallocated memory that are individually too small to hold new programs (**External Fragmentation**).
**Paging** eliminates external fragmentation by breaking virtual memory into fixed-size chunks called **Pages** (typically 4 KB) and physical memory into identical fixed-size blocks called **Frames**. Any virtual page can be mapped to *any* available physical frame anywhere in RAM, removing the requirement that memory be physically contiguous.

---

### Q5: What is a Page Fault, and what sequence of events occurs when one is triggered?
**Answer:**
A Page Fault is a hardware trap raised by the CPU's Memory Management Unit (MMU) when a program accesses a virtual memory address whose page table entry has the **Present bit = 0**.
1. The CPU halts the faulting instruction and switches execution to the kernel in Ring 0.
2. The kernel verifies whether the address is valid:
   - If invalid/illegal ➔ Generates `SIGSEGV` (Segmentation Fault) and terminates the program.
3. If valid, the kernel locates a free physical frame in RAM.
4. The kernel issues a disk I/O request to load the missing page from disk (swap or executable file).
5. The thread sleeps while I/O completes.
6. Once loaded into RAM, the kernel updates the process's page table with the new physical frame number and sets the Present bit to 1.
7. The CPU restarts the exact instruction that originally caused the fault.

---

### Q6: Name and explain the 4 Coffman Conditions required for a Deadlock to occur.
**Answer:**
A deadlock in any system can occur if and only if all four conditions hold simultaneously:
1. **Mutual Exclusion:** At least one resource must be held in a non-shareable mode (exclusive access).
2. **Hold and Wait:** A process is holding at least one resource while waiting to acquire additional resources held by other processes.
3. **No Preemption:** Resources cannot be forcibly taken from a process; they can only be released voluntarily.
4. **Circular Wait:** A closed cycle exists: Process 1 waits for a resource held by Process 2, which waits for a resource held by Process 3, which waits for Process 1.

---

### Q7: What is the difference between Hard Real-Time and Soft Real-Time Operating Systems?
**Answer:**
- **Hard Real-Time:** Strict deterministic timing guarantees where missing a deadline constitutes a total, catastrophic system failure (e.g., flight control systems, automotive ABS brakes, medical ventilators). Examples: VxWorks, QNX.
- **Soft Real-Time:** Missing a deadline degrades performance or service quality, but does not cause catastrophic destruction (e.g., live video streaming, online audio playback). Examples: Standard Linux with the `PREEMPT_RT` patch.

---

### Q8: Why can Linux Load Average be high when CPU utilization is near zero?
**Answer:**
In Linux, the Load Average metric measures the total number of threads that are:
1. Running on a CPU (`R`).
2. Waiting in the CPU ready run-queue (`R`).
3. **Sleeping in Uninterruptible Disk I/O State (`D`).**
If a storage array, SAN volume, or network NFS share hangs, dozens of processes can enter state `D` waiting for disk blocks to return. Because they are in state `D`, they contribute directly to the Load Average, even though the CPU cores are 100% idle!

---

### Q9: What is the difference between Monolithic, Microkernel, and Hybrid Operating Systems?
**Answer:**
- **Monolithic (Linux):** All core services (scheduler, virtual memory, VFS, networking, and device drivers) run together inside privileged CPU Ring 0. High performance, but driver bugs can cause a kernel panic.
- **Microkernel (MINIX, seL4):** Only the bare minimum (IPC, basic scheduling, basic memory) runs in Ring 0. Drivers and filesystems run as isolated user space server processes (Ring 3). High reliability, but higher IPC context switch overhead.
- **Hybrid (Windows NT, macOS XNU):** Combines microkernel architecture with monolithic performance by running some subsystem servers in user space while executing performance-critical subsystems (graphics, drivers) in kernel space.

---

### Q10: Differentiate between Resident Set Size (RSS) and Virtual Memory Size (VSZ).
**Answer:**
- **VSZ (Virtual Memory Size):** The total virtual address space allocated to the process, including mapped shared libraries, executable code, stack, and uncommitted heap allocations.
- **RSS (Resident Set Size):** The actual number of physical RAM pages currently mapped and resident in DRAM memory frames for that process.
- *Production Significance:* High VSZ is normal and harmless. A continuously increasing RSS indicates an active memory leak that will eventually trigger the kernel's Out-Of-Memory (OOM) Killer.
