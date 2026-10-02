# 15 — Technical Interview Questions & Answers (Junior to Staff SRE)

A rigorous compilation of technical interview questions testing operating systems, kernel architecture, memory management, processes, signals, and container isolation.

---

### Q1: What happens at the CPU hardware level when a user space program makes a system call?
**Level:** Intermediate / Senior  
**Answer:**
1. **Prepare Arguments:** The application (or standard library wrapper) loads the system call number into the architecture's designated register (`RAX` on x86_64) and function arguments into parameter registers (`RDI`, `RSI`, `RDX`, `R10`, `R8`, `R9`).
2. **Execute Trap Instruction:** The CPU executes the `syscall` instruction (or `svc` on ARM64).
3. **Privilege Transition:** The CPU switches its hardware privilege ring from **Ring 3 (User Mode)** to **Ring 0 (Kernel Mode)**.
4. **Stack Swap:** The CPU switches the Stack Pointer (`RSP`) from the process's user stack to its dedicated kernel stack.
5. **Jump to Kernel Entry:** The CPU saves the return instruction address (`RIP`) into `RCX` and jumps to the system call dispatcher address stored in the Model-Specific Register `MSR_LSTAR`.
6. **Dispatch & Execution:** The kernel verifies memory pointers, checks security policies, executes the kernel function from `sys_call_table`, and writes the result code into `RAX`.
7. **Privilege Drop:** The kernel executes `sysret`, restoring CPU state to Ring 3 and resuming user program execution.

---

### Q2: Explain the Copy-On-Write (COW) mechanism during `fork()`. Why is it critical for container performance?
**Level:** Intermediate  
**Answer:**
- In legacy Unix, `fork()` physically duplicated all RAM pages of the parent process into the child, which was slow and consumed double the physical memory.
- Under modern **Copy-On-Write (COW)**:
  1. The kernel creates a new task structure and duplicates the **page table entries**, pointing both parent and child page tables to the **exact same physical RAM frames**.
  2. The kernel marks all shared pages as **Read-Only** in both page tables.
  3. Neither process experiences memory duplication at creation time; `fork()` executes in microseconds.
  4. If either process attempts to write to a page, the CPU MMU detects a write to a read-only page and triggers a **Minor Page Fault**.
  5. The kernel catches the fault, allocates a new physical 4 KB frame, copies only that single page's contents, updates the writing process's page table to point to the new frame with read-write permissions, and transparently resumes execution.
- **Container Impact:** When containers spin up worker processes or run CLI commands, COW guarantees near-instantaneous startup without duplicating shared binary libraries in memory.

---

### Q3: Why can't a Zombie process be killed with `kill -9`, and how do you properly resolve a zombie leak?
**Level:** Junior / Intermediate  
**Answer:**
- **Why `kill -9` fails:** A Zombie process (`state Z` / `defunct`) has already exited and is physically dead. Its memory, heap, and open file descriptors have already been released back to the OS. The only thing remaining is its entry in the kernel's process table (`task_struct`), keeping its PID and exit status alive until the parent process calls `wait()`. Because the process has no running code, it cannot receive signals.
- **Resolution:**
  1. Send `SIGCHLD` to the parent process to remind it to invoke `wait()`:
     ```bash
     kill -s SIGCHLD <parent_pid>
     ```
  2. If the parent process is hung or poorly written, terminate the **parent process**:
     ```bash
     kill -9 <parent_pid>
     ```
     Once the parent dies, the zombie is orphaned and automatically adopted by **PID 1 (`systemd`)**. PID 1 continuously executes `wait()`, immediately reaping and clearing the zombie from the process table.

---

### Q4: What is the architectural difference between Linux Namespaces and Control Groups (cgroups)?
**Level:** Senior  
**Answer:**
- **Linux Namespaces provide ISOLATION (Visibility):**
  - Namespaces control what a process can **see**.
  - A process inside a PID namespace only sees its own process tree (seeing itself as PID 1).
  - A process inside a Network namespace has its own loopback interface, routing tables, and port bindings.
- **Linux Control Groups (cgroups) provide RESOURCE CONSTRAINTS (Metering & Limits):**
  - cgroups control how much a process can **use**.
  - Enforces hard limits and quotas on CPU time slices, physical RAM consumption, swap boundaries, block I/O throughput, and maximum PIDs.
- **Summary:** Namespaces make a process believe it is the only application running on the entire computer. cgroups ensure that process does not monopolize the host's hardware resources.

---

### Q5: What is the difference between Minor, Major, and Invalid Page Faults?
**Level:** Intermediate / Senior  
**Answer:**
- **Minor Page Fault:** The page data is already resident in physical RAM (e.g., shared library mapped by another process, or an anonymous memory page allocated by `malloc`), but the process's page table entry is not yet established. The kernel updates the page table without disk I/O (resolves in microseconds).
- **Major Page Fault:** The requested memory page is not in physical RAM; it is located on secondary storage (swapped to disk, or part of a binary/shared library file). The kernel halts the process, issues synchronous disk I/O, loads the page into RAM, updates the page table, and resumes the process (resolves in milliseconds).
- **Invalid Page Fault:** The process attempted to access an address outside its virtual address space, or attempted to write to a read-only page (e.g., writing to the code text segment). The MMU raises an exception, and the kernel terminates the process with `SIGSEGV` (Segmentation Fault).

---

### Q6: Why are Unix Domain Sockets (UDS) faster than TCP loopback (`127.0.0.1`) for local IPC?
**Level:** Senior  
**Answer:**
1. **No Network Protocol Overhead:** Unix Domain Sockets bypass the entire TCP/IP network stack. There is no IP packet framing, no checksum calculations, no sequence numbers, no ACK packets, and no TCP sliding window flow control.
2. **Zero Context Switching for Protocol Checks:** Data is transferred directly through kernel memory buffers.
3. **File Descriptor Passing:** Unix Domain Sockets support passing open file descriptors across process boundaries via `SCM_RIGHTS`, which is impossible over TCP.
4. **Security:** Authorization is checked using native Linux filesystem permissions (UID/GID) rather than network firewall rules.

---

### Q7: Explain the difference between `SIGTERM` (15) and `SIGKILL` (9). Why does Kubernetes use both?
**Level:** Junior / Intermediate  
**Answer:**
- **`SIGTERM` (Signal 15):** The standard **graceful termination** signal. It can be intercepted, handled, or blocked by the application. Well-architected services catch `SIGTERM` to close active network sockets, stop accepting new HTTP requests, finish processing in-flight database transactions, flush log buffers to disk, and exit cleanly with code 0.
- **`SIGKILL` (Signal 9):** The **forced immediate termination** signal. It cannot be caught, handled, or ignored by user space. The kernel immediately seizes the process's execution thread, destroys its memory space, and frees its file descriptors.
- **Kubernetes Lifecycle:** When a Pod is terminated, Kubernetes sends `SIGTERM` and initiates a countdown timer (`terminationGracePeriodSeconds`, default 30s). This gives the application time to shut down gracefully. If the pod has not exited when the timer expires, Kubernetes sends `SIGKILL` to prevent hanging processes from blocking cluster updates.

---

### Q8: What is an Inode, and what happens at the filesystem level when you run `rm file.txt`?
**Level:** Intermediate  
**Answer:**
- An **Inode** is a filesystem data structure that stores all metadata about a file (size, permissions, owner, timestamps, and physical data block pointers) **except for its filename**.
- When you run `rm file.txt`:
  1. The kernel invokes the `unlink()` system call.
  2. The directory entry mapping the name `"file.txt"` to its inode number is removed from the directory.
  3. The kernel decrements the inode's **Hard Link Count** by 1.
  4. If the hard link count reaches `0` **AND no running process holds an open file descriptor to that inode**, the kernel marks the inode and its associated physical disk blocks as free in the filesystem allocation bitmap.
  5. If a process still has the file open, the disk blocks remain allocated and consumed until that process terminates or closes the file descriptor.

---

### Q9: What is eBPF, and how does it differ from a traditional Loadable Kernel Module (LKM)?
**Level:** Senior / Staff SRE  
**Answer:**
- **Kernel Module (LKM):** Written in C, compiled against specific kernel headers, and loaded into Ring 0. A single bug (e.g., null-pointer dereference or infinite loop) in a kernel module causes an immediate **Kernel Panic**, crashing the physical host.
- **eBPF (Extended Berkeley Packet Filter):**
  - Runs inside an **In-Kernel Virtual Machine**.
  - **In-Kernel Verifier:** Before any eBPF bytecode is permitted to run, the kernel verifier proves mathematically that the program contains no infinite loops, cannot access uninitialized memory, cannot crash the kernel, and will terminate within bounded instructions.
  - **Safety & Portability:** Can be loaded and updated at runtime with zero downtime and zero risk of host crashes.
  - **Performance:** JIT-compiled into native CPU instructions, delivering performance comparable to native kernel code.

---

### Q10: How does `cgroups v2` differ from `cgroups v1`, and why did Kubernetes migrate to v2?
**Level:** Senior / Staff SRE  
**Answer:**
- **cgroups v1** allowed multiple independent, uncoordinated hierarchies (`/sys/fs/cgroup/cpu`, `/sys/fs/cgroup/memory`). This caused severe bugs:
  - Buffered writeback I/O in the page cache could not be attributed back to the memory-cgroup that generated it, breaking block I/O throttling.
  - Confusing API with multiple conflicting configuration files.
- **cgroups v2** implements a **Single Unified Hierarchy**:
  - All controllers (CPU, Memory, I/O, PID) are bound to the same process tree node.
  - Introduces **Pressure Stall Information (PSI)** to detect resource saturation before crashes happen.
  - Introduces **`memory.high`** for soft memory throttling (reclaim memory gracefully) alongside **`memory.max`** (hard OOM kill ceiling).
  - Modern Kubernetes (v1.25+) relies on cgroups v2 for accurate container resource accounting and memory QoS.

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [14 - Troubleshooting](./14-Troubleshooting.md) | [README](./README.md) | [16 - Hands On Practice](./16-Hands-On-Practice.md) |
