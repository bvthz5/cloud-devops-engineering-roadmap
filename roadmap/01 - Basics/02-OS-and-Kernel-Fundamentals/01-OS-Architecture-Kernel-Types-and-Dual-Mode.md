# 01 — Operating System Architecture, Kernel Models, and Dual-Mode Operation

---

## 1. Operating System Fundamentals

An **Operating System (OS)** is system software that manages computer hardware, software resources, and provides common services for application programs. It acts as an intermediary layer between bare-metal hardware and user applications.

```text
+-----------------------------------------------------------+
|               User Applications (Nginx, Python, Node)      |
+-----------------------------------------------------------+
|               C Standard Library (glibc / musl)           |
+-----------------------------------------------------------+
|               System Call Interface (syscall)             |
+-----------------------------------------------------------+
|               OPERATING SYSTEM KERNEL                     |
|  - Process Scheduler    - Virtual Memory Manager          |
|  - VFS Filesystem       - Network Stack (TCP/IP)          |
|  - Device Drivers       - Security & IPC                  |
+-----------------------------------------------------------+
|               Hardware Architecture                       |
|  - CPU & Caches         - Physical RAM                    |
|  - Storage (NVMe/SSD)   - Network Interfaces (NIC)        |
+-----------------------------------------------------------+
```

---

## 2. Core OS Responsibilities

1. **Process Management:** Creating, scheduling, synchronizing, and terminating processes and threads across CPU cores.
2. **Memory Management:** Allocating and deallocating memory, managing virtual-to-physical address translation, paging, and protecting process memory spaces.
3. **Storage & Filesystem Management:** Managing directory structures, block storage abstraction, filesystems (ext4, XFS, Btrfs), and caching disk I/O.
4. **Device Management:** Providing standardized hardware abstraction through device drivers (character, block, and network devices).
5. **Security & Protection:** Enforcing user privileges, resource isolation (namespaces/cgroups), access control lists, and security policies (SELinux/AppArmor).
6. **Networking:** Implementing network protocols (Ethernet, IP, TCP, UDP), managing routing tables, packet filtering, and socket abstractions.

---

## 3. The Kernel: The Core of the Operating System

The **Kernel** is the essential, resident core of the operating system that runs with the highest hardware privileges. It is loaded into memory by the bootloader during system startup and remains in RAM until the system is powered off.

### Key Characteristics of the Kernel
- It has direct, unrestricted access to all CPU instructions, physical memory chips, and hardware devices.
- It provides a safe, mediated abstraction layer so that user processes cannot directly manipulate raw hardware or crash other processes.
- A crash inside the kernel (such as a null-pointer dereference in kernel code) results in a catastrophic system crash (**Kernel Panic** in Linux or **Blue Screen of Death / BSOD** in Windows).

---

## 4. Kernel Architectures Comparison

Operating system kernels are categorized by how services (filesystems, drivers, network stacks) are partitioned between privileged space and user space.

```text
MONOLITHIC KERNEL (Linux)                 MICROKERNEL (Mach, seL4)
+-------------------------------+       +-------------------------------+
| User Applications (User Space)|       | Apps | FS Server | Net Server | (User Space)
+===============================+       +===============================+
| KERNEL SPACE (Ring 0)         |       | KERNEL SPACE (Ring 0)         |
|  - Scheduler   - IPC          |       |  - Basic IPC                  |
|  - Memory Mgr  - VFS          |       |  - Low-level Scheduling       |
|  - Net Stack   - Drivers      |       |  - Minimal Address Space Mgmt |
+-------------------------------+       +-------------------------------+
```

### Detailed Architecture Comparison Table

| Attribute | Monolithic Kernel | Microkernel | Hybrid Kernel |
| :--- | :--- | :--- | :--- |
| **Examples** | Linux, BSD, Solaris | seL4, QNX, Minix, GNU Mach | Windows NT, macOS (XNU) |
| **Location of Drivers & FS** | Inside Kernel Space (Ring 0) | User Space processes | Mostly Kernel Space for speed |
| **Performance** | **Extremely High** (direct in-memory function calls) | **Lower** (heavy IPC & context switches between servers) | **Balanced** (near-monolithic speed) |
| **Fault Tolerance** | A bug in a driver can crash the entire kernel | Crashed driver process can be restarted without kernel panic | Unstable drivers can crash the OS |
| **Extensibility** | Loadable Kernel Modules (LKM) | Modular user servers | Dynamically loaded kernel drivers |
| **Codebase Size** | Millions of lines (Linux > 30M lines) | Minimal (seL4 ~ 10,000 lines, mathematically proven) | Large |

### Why Linux Chose Monolithic Architecture
Linus Torvalds famously debated Andrew Tanenbaum (creator of Minix) in 1992. Torvalds argued that microkernels suffered from severe performance degradation caused by constant context switches and message passing across address boundaries. Linux achieved unmatched speed by running drivers, networking, and memory management inside a single address space, while maintaining modularity through **Loadable Kernel Modules (LKM)**.

---

## 5. Dual-Mode Operation: User Space vs Kernel Space

CPUs implement hardware protection rings to prevent user applications from compromising system integrity. Modern x86_64 and ARM64 architectures enforce two primary operating states:

```text
Hardware Privilege Rings (x86_64)

       Ring 3: User Space (Applications, Shells, Daemons)
         ▲
         │ (syscall trap)
         ▼
       Ring 0: Kernel Space (Kernel Core, Device Drivers, Scheduler)
```

### Ring 3: User Mode / User Space
- **Restricted Privileges:** Cannot execute sensitive CPU instructions (such as `cli` to disable interrupts, `wrmsr` to modify model-specific registers, or `mov %cr3` to alter page tables).
- **Isolated Virtual Memory:** Each process sees only its own private virtual address space (e.g., lower 47 bits on x86_64 canonical addressing). It cannot read or write to kernel memory or another process's memory.
- **Hardware Fault Isolation:** If an application crashes (e.g., divides by zero or dereferences an invalid pointer), the CPU generates an exception and the kernel terminates only that process (`SIGSEGV`), leaving the rest of the OS completely unaffected.

### Ring 0: Privileged Mode / Kernel Space
- **Unrestricted Privileges:** Can execute any valid machine instruction, access every physical RAM address, and communicate directly with hardware I/O ports and memory-mapped I/O (MMIO) registers.
- **Shared Memory Space:** The kernel address space is mapped into the upper portion of every process's virtual memory map (protected by page table supervisor bits).

---

## 6. Privilege Switching: Crossing the Boundary

How does a user space program write to a disk or send a packet over the network if it lacks hardware access?

1. The user program invokes a C standard library function (e.g., `write()`).
2. The library places the system call number in a CPU register (e.g., `RAX = 1` for `sys_write` on x86_64) and loads function arguments into registers (`RDI`, `RSI`, `RDX`).
3. The CPU executes the special **`syscall`** instruction (on x86_64) or **`svc`** (on ARM64).
4. The CPU hardware:
   - Switches privilege level from **Ring 3** to **Ring 0**.
   - Switches the Stack Pointer register (`RSP`) from the User Stack to the Kernel Stack.
   - Jumps to the pre-configured kernel system call handler address stored in the Model-Specific Register (`MSR_LSTAR`).
5. The kernel validates the arguments, executes the requested action, writes the return value to `RAX`, and executes **`sysret`** to drop privileges back to **Ring 3**.

---

## DevOps Key Takeaway
Every time a containerized app handles an HTTP request, logs to disk, or queries a database, it makes hundreds of transitions between User Space and Kernel Space. Understanding where the boundary lies is fundamental to profiling CPU usage (`%usr` vs `%sys` in `top`), diagnosing permission errors, and understanding container isolation.

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [README (Index)](./README.md) | [README](./README.md) | [02 - System Calls Interface and Categories](./02-System-Calls-Interface-and-Categories.md) |
