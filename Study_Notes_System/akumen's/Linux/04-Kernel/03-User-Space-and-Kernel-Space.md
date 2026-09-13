# 03 - User Space and Kernel Space: The Privilege Divide

The fundamental boundary that keeps a Linux system secure and crash-resilient is the strict separation between **User Space** and **Kernel Space**.

---

## 1. The Architectural Divide

```
┌─────────────────────────────────────────────────────────────┐
│                         USER SPACE                          │
│ • Runs in CPU Ring 3 (Unprivileged User Mode)               │
│ • User applications, web servers, shells, background daemons│
│ • Access to hardware devices is STRICTLY PROHIBITED         │
│ • Memory access is restricted to its own virtual address    │
│ • A crash = Process Segfault (Rest of the system survives)  │
└──────────────────────────────┬──────────────────────────────┘
                               │ System Call (`syscall` instruction)
═══════════════════════════════╪═══════════════════════════════
                               ▼ (CPU switches privilege to Ring 0)
┌─────────────────────────────────────────────────────────────┐
│                        KERNEL SPACE                         │
│ • Runs in CPU Ring 0 (Privileged Supervisor Mode)           │
│ • Core kernel, scheduler, memory manager, device drivers    │
│ • UNRESTRICTED access to physical memory & hardware ports   │
│ • Executes privileged machine instructions (e.g. CLI, HLT)  │
│ • A crash = KERNEL PANIC (The entire machine halts!)        │
└─────────────────────────────────────────────────────────────┘
```

---

## 2. Virtual Memory Address Partitioning (x86_64)

In modern 64-bit Linux, the CPU uses a 48-bit (or 57-bit) virtual addressing space, canonicalized into two distinct halves:

```
0xFFFFFFFFFFFFFFFF ┌─────────────────────────────────────────┐
                   │               KERNEL SPACE              │
                   │   (128 TB - Shared kernel mappings,     │
                   │    device drivers, kernel code & stack) │
0xFFFF800000000000 ├─────────────────────────────────────────┤
                   │       Non-Canonical Address Hole        │
                   │               (Unmapped)                │
0x00007FFFFFFFFFFF ├─────────────────────────────────────────┤
                   │                USER SPACE               │
                   │   (128 TB - Private to this process:    │
                   │    Stack, Heap, Mapped Libraries, Code) │
0x0000000000000000 └─────────────────────────────────────────┘
```

- **Lower Half (`0x0000...` to `0x00007FFF...`):** Belongs to the currently running **User Space process**. Each process has its own unique page table for this region.
- **Upper Half (`0xFFFF8000...` to `0xFFFF...`):** Reserved exclusively for **Kernel Space**. It maps identical kernel memory across all processes, but user space programs are forbidden by the MMU hardware from reading or writing to these addresses.

---

## 3. Fault Containment: Segmentation Fault vs. Kernel Panic

| Failure Type | Where It Happens | What Happens | System Outcome |
|---|---|---|---|
| **Segmentation Fault (`SIGSEGV`)** | **User Space** (Ring 3) | A program attempts to read or write an invalid or unmapped memory address. | The CPU raises an exception; the kernel terminates the offending process. **Zero impact on other processes.** |
| **Kernel Panic** | **Kernel Space** (Ring 0) | The kernel encounters an unrecoverable error (e.g., null pointer in a driver, corrupted core data structure). | The kernel halts CPU execution, dumps registers and stack traces to the console, and **stops the entire machine**. |

---

## 4. Kernel Page Table Isolation (KPTI)

In 2018, researchers discovered the **Meltdown** hardware vulnerability, which allowed malicious user space code to use speculative CPU execution to read privileged kernel memory.
In response, the Linux kernel implemented **KPTI (Kernel Page Table Isolation)**:
- In user mode, the kernel's memory map is stripped out of the process's page table entirely.
- When a system call occurs, the kernel swaps to a separate page table containing kernel memory, eliminating speculative leaks at the cost of a slight performance penalty on older CPUs.
