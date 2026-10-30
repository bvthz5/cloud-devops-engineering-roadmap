# 05 — OS Memory Management, Paging, Swap, and mmap

---

## 1. Operating System Memory Management

The memory management subsystem of the Linux kernel is responsible for:
- Allocating physical memory to user space processes and the kernel core.
- Providing each process with a contiguous, isolated **Virtual Address Space**.
- Dynamically paging inactive memory to secondary storage (Swap).
- Caching disk blocks in unused RAM to accelerate storage I/O (**Page Cache**).
- Reclaiming and recycling memory when system allocations reach critical thresholds.

```text
Process Virtual Memory Map (Canonical 64-bit Linux)
0xFFFFFFFFFFFFFFFF ┌────────────────────────────────────────┐
                   │ Kernel Space (Protected Ring 0)        │
0xFFFF800000000000 ├────────────────────────────────────────┤
                   │ [Non-Canonical Hole]                  │
0x00007FFFFFFFFFFF ├────────────────────────────────────────┤
                   │ User Stack (grows downwards)           │
                   │   ▼                                    │
                   ├────────────────────────────────────────┤
                   │ Memory-Mapped Files & Libraries (mmap) │
                   ├────────────────────────────────────────┤
                   │   ▲                                    │
                   │ Heap (grows upwards via brk/sbrk)      │
                   ├────────────────────────────────────────┤
                   │ BSS (Uninitialized Global Data)        │
                   ├────────────────────────────────────────┤
                   │ Data Segment (Initialized Globals)     │
                   ├────────────────────────────────────────┤
                   │ Text Segment (Binary Executable Code)  │
0x0000000000000000 └────────────────────────────────────────┘
```

---

## 2. Memory Allocation in User Space: Heap vs Stack

| Property | Stack Allocation | Heap Allocation |
| :--- | :--- | :--- |
| **Purpose** | Local variables, function call frames, return addresses | Dynamic, long-lived data structures (objects, arrays) |
| **Management** | Automatic by CPU (adjusting `RSP` stack pointer register) | Manual by programmer / runtime (`malloc()`, `free()`, GC) |
| **Allocation Speed**| Extremely fast (~1 clock cycle: `sub $32, %rsp`) | Slower (searches free lists, fragmentation checks) |
| **Underlying Syscall**| None (pre-allocated stack segment) | **`brk()`** (extends data segment) or **`mmap()`** |
| **Failure Mode** | **Stack Overflow** (`SIGSEGV` when hitting guard page) | **Out Of Memory (OOM)** (`malloc` returns NULL or kills app) |

### How `malloc()` Works Internally
The standard C library (`glibc` ptmalloc):
- For small allocations (< 128 KB): Uses **`brk()`** or **`sbrk()`** to move the heap boundary pointer up and down.
- For large allocations (>= 128 KB): Uses **`mmap()`** to allocate an anonymous memory mapping directly from the kernel, which is returned to the OS immediately upon `free()`.

---

## 3. Paging & Multi-Level Page Tables

To eliminate physical memory fragmentation, modern operating systems divide memory into fixed-size chunks:
- **Pages:** Blocks of virtual memory (typically **4 KB**).
- **Page Frames:** Physical slots of equal size in DRAM chips.

### Multi-Level Page Tables on x86_64 (4-Level Paging)
Directly mapping 64-bit memory in a flat table would require petabytes of RAM just for the page table itself. Linux uses a hierarchical **4-level (or 5-level with 57-bit addressing)** tree structure:

```text
Virtual Address (48 bits):
[ 9 bits: PGD ] [ 9 bits: P4D ] [ 9 bits: PUD ] [ 9 bits: PMD ] [ 9 bits: PTE ] [ 12 bits: Page Offset ]
       │               │               │               │               │                  │
       ▼               ▼               ▼               ▼               ▼                  │
    [CR3] ──► Page Directory ──► Page Upper ──► Page Middle ──► Page Table ──► Physical Frame
                                                                                   │
                                                     Physical Memory Address ◄─────┘
```
The **`CR3` register** on the CPU stores the physical pointer to the top-level Page Global Directory (PGD). When the OS switches processes, it simply changes the value in `CR3`.

---

## 4. Page Faults Demystified

A **Page Fault** is a CPU hardware interrupt triggered when an executing program attempts to access a virtual memory page that is not currently mapped into physical RAM.

```text
                        [ CPU accesses Virtual Address ]
                                       │
                                       ▼
                             Is Page in MMU TLB?
                             ├── Yes ──► Direct RAM Access (1 ns)
                             └── No
                                       │
                                       ▼
                       Does Valid Page Table Entry Exist?
                       ├── No ──► [ INVALID PAGE FAULT / SIGSEGV ]
                       │          (Segmentation fault: Core dump)
                       └── Yes
                                       │
                                       ▼
                       Is Present Bit = 1 in Page Table?
                       ├── Yes ──► [ MINOR PAGE FAULT ]
                       │           (Allocated page, updates TLB)
                       └── No
                                       │
                                       ▼
                            [ MAJOR PAGE FAULT ]
                    (Page swapped out or file-backed;
                     Kernel pauses process, reads from Disk)
```

1. **Minor Page Fault:** The page data is already resident in RAM (e.g., shared library loaded by another process, or freshly allocated zeroed page), but the process's page table has not yet linked to it. Resolved quickly in microseconds.
2. **Major Page Fault:** The requested data resides on secondary storage (swapped to disk or inside an executable file). The CPU must block the process and wait for disk I/O (milliseconds).
3. **Invalid Page Fault (SIGSEGV):** The address is outside the process's allocated virtual space or violates permissions (e.g., writing to a read-only code page). The kernel sends `SIGSEGV` (Signal 11), crashing the process.

---

## 5. Swapping & Kernel Swappiness

**Swapping** is the mechanism where the Linux kernel moves inactive anonymous memory pages from physical RAM into a dedicated disk partition or swap file when RAM becomes constrained.

### Understanding `vm.swappiness`
The `vm.swappiness` kernel parameter (range `0` to `200`, default `60`) dictates how aggressively the kernel swappiness algorithm prefers reclaiming file-backed Page Cache vs swapping out anonymous memory.

```text
vm.swappiness = 0   : Avoid swapping anonymous memory unless completely out of RAM.
vm.swappiness = 60  : Balanced strategy (default on desktop/general servers).
vm.swappiness = 1   : Minimum swapping without fully disabling it (Recommended for DBs).
vm.swappiness = 100 : Equal aggressiveness between reclaiming page cache and swapping.
```

### Checking and Tuning Swappiness
```bash
# Check current swappiness
cat /proc/sys/vm/swappiness

# Set temporarily to 10 for low-latency database nodes
sudo sysctl -w vm.swappiness=10

# Persist across reboots in /etc/sysctl.d/99-sysctl.conf
echo "vm.swappiness=10" | sudo tee -a /etc/sysctl.d/99-sysctl.conf
```

> **Kubernetes Note:** Historically, Kubernetes required swap to be completely disabled (`swapoff -a`) so the Kubelet could enforce deterministic memory limits. Starting in Kubernetes v1.28+, swap is supported in beta for specific memory-overcommit node tiers.

---

## 6. Memory-Mapped Files (`mmap`)

The **`mmap()`** system call maps a file directly into a process's virtual address space.

```text
Traditional File I/O (read/write):
[ Disk File ] ──(DMA)──► [ Kernel Page Cache ] ──(Copy)──► [ User Buffer in RAM ]
* Incurs 2 memory copies and frequent syscalls.

Memory-Mapped I/O (mmap):
[ Disk File ] ──(DMA)──► [ Physical RAM Frame ] ◄────────── [ User Process Pointer ]
* Zero user-space copying; pointer dereferences read directly from page cache.
```

### Real-World Use Cases
- **Databases:** RocksDB, MongoDB, SQLite, and Kafka (for index offsets) use `mmap` for ultra-fast, zero-copy disk data access.
- **Dynamic Linker:** `ld-linux.so` maps shared libraries (`.so` files) into process address spaces with read-only execution flags, allowing thousands of running containers to share the exact same physical copy of `libc.so` in RAM.
- **Anonymous Memory:** Allocating large memory buffers (> 128 KB) without an underlying file.

---

## 7. Memory Protection: ASLR and the NX Bit

To prevent exploitation by buffer overflows, modern kernels enforce strict hardware-assisted memory security:
1. **NX Bit (No-Execute) / W^X (Write XOR Execute):**
   A page can either be **Writable** or **Executable**, but never both. This prevents an attacker from injecting shellcode into the stack/heap and executing it.
2. **ASLR (Address Space Layout Randomization):**
   Randomizes the memory offsets of the stack, heap, and shared libraries on every program execution.
   ```bash
   # Check ASLR status (2 = Full randomization)
   cat /proc/sys/kernel/randomize_va_space
   ```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 04 - Threads Multithreading and CPU Scheduling](./04-Threads-Multithreading-and-CPU-Scheduling.md) | [Index](../../../README.md) | [06 - Filesystem Architecture Inodes and File Descriptors →](./06-Filesystem-Architecture-Inodes-and-File-Descriptors.md) |
