# 05 - How File Copy Works: Tracing `cp` Through All 5 Layers

The classic benchmark for understanding Linux architecture is tracing what occurs under the hood when you execute a simple command:
```bash
$ cp source.txt destination.txt
```
This single operation traverses all five architectural layers, executing thousands of CPU instructions, multiple mode switches, memory allocations, and hardware interrupts.

---

## 🗺️ End-to-End Architectural Trace Diagram

```
┌─────────────────────────────────────────────────────────────────────────────┐
│ 1. USER & SHELL (Layers 5 & 4)                                              │
│ • User hits [ENTER] on: "cp source.txt destination.txt"                     │
│ • Shell parses tokens, finds /usr/bin/cp in $PATH                          │
│ • Shell invokes: fork() ➔ execve("/usr/bin/cp", ...)                       │
└──────────────────────────────────────┬──────────────────────────────────────┘
                                       │
┌──────────────────────────────────────▼──────────────────────────────────────┐
│ 2. DYNAMIC LINKER & SYSTEM LIBRARIES (Layer 3)                              │
│ • ld-linux loads /lib/x86_64-linux-gnu/libc.so.6 into memory                │
│ • glibc initializes memory buffers and wraps syscall invocations            │
└──────────────────────────────────────┬──────────────────────────────────────┘
                                       │ System Call Boundary
═══════════════════════════════════════╪═══════════════════════════════════════
                                       │ Mode Switch: Ring 3 ➔ Ring 0
┌──────────────────────────────────────▼──────────────────────────────────────┐
│ 3. SYSTEM CALL INVOCATIONS (Kernel Entry)                                   │
│ • openat(AT_FDCWD, "source.txt", O_RDONLY) ─────────────> Returns fd: 3     │
│ • openat(AT_FDCWD, "destination.txt", O_WRONLY|O_CREAT) > Returns fd: 4     │
│ • read(3, buffer, 131072) / write(4, buffer, count)                        │
│   (or modern kernel-level: copy_file_range(3, ..., 4, ...))                 │
└──────────────────────────────────────┬──────────────────────────────────────┘
                                       │
┌──────────────────────────────────────▼──────────────────────────────────────┐
│ 4. KERNEL SUBSYSTEMS (Layer 2)                                              │
│ • VFS (Virtual Filesystem): Resolves path string to Inode & checks ACLs     │
│ • Memory Management & Page Cache: Allocates physical RAM pages; marks      │
│   destination pages as "dirty"                                              │
│ • Block Layer & I/O Scheduler: Coalesces contiguous disk block requests     │
│ • Device Driver: Prepares NVMe/SATA Direct Memory Access (DMA) commands     │
└──────────────────────────────────────┬──────────────────────────────────────┘
                                       │
┌──────────────────────────────────────▼──────────────────────────────────────┐
│ 5. PHYSICAL HARDWARE (Layer 1)                                              │
│ • PCIe Controller transfers data via DMA from RAM directly to SSD           │
│ • NVMe Controller writes bits to NAND Flash memory                          │
│ • Hardware Interrupt (IRQ) sent from SSD to CPU notifying I/O completion    │
└─────────────────────────────────────────────────────────────────────────────┘
```

---

## 🔬 Detailed Step-by-Step Breakdown

### Step 1: Shell Interpretation & Process Creation
1. The terminal emulator receives keystrokes via character device `/dev/pts/0`.
2. The interactive shell reads the line buffer.
3. The shell checks internal builtins (`cd`, `export`); since `cp` is an external disk binary, it scans directories in `$PATH`.
4. The shell invokes the **`fork()`** system call, cloning itself into a new child process.
5. Inside the child process, it invokes **`execve("/usr/bin/cp", ["cp", "source.txt", "destination.txt"], envp)`**.

### Step 2: Binary Initialization & Library Binding
1. The Linux kernel ELF loader parses the `/usr/bin/cp` binary headers.
2. The dynamic linker (`ld-linux.so`) maps required shared libraries (principally `libc.so.6`) into the process's virtual memory space.
3. The program entry point (`main`) begins execution in User Space (Ring 3).

### Step 3: Opening Files & Allocating File Descriptors
To read or write, the process needs integer handles called **File Descriptors (FD)**:
1. `cp` invokes `openat()` requesting read access to `source.txt`:
   ```c
   int fd_in = openat(AT_FDCWD, "source.txt", O_RDONLY);
   ```
   - The kernel validates that `source.txt` exists and the user has read permission (`r`).
   - If successful, the kernel allocates **FD 3** in the process file descriptor table.
2. `cp` invokes `openat()` to create or truncate `destination.txt`:
   ```c
   int fd_out = openat(AT_FDCWD, "destination.txt", O_WRONLY|O_CREAT|O_TRUNC, 0666);
   ```
   - The kernel allocates a new inode on the destination filesystem and assigns **FD 4**.

### Step 4: The Read & Write Loop (Or `copy_file_range`)

#### The Traditional User Space Bounce-Buffer Method:
Historically, `cp` allocated a buffer in RAM (e.g., 128 KB) and looped:
```c
while ((bytes = read(fd_in, buffer, 131072)) > 0) {
    write(fd_out, buffer, bytes);
}
```
*Disadvantage:* Every chunk crossed the user space/kernel space boundary twice (`read` switch + `write` switch), copying data through user memory.

#### The Modern Linux Zero-Copy Method (`copy_file_range`):
On modern Linux kernels (v4.5+), GNU `cp` uses the specialized **`copy_file_range`** system call:
```c
copy_file_range(fd_in, NULL, fd_out, NULL, 1048576, 0);
```
This tells the kernel to transfer data directly from one file to another **entirely inside kernel space (or via filesystem Reflink on Btrfs/XFS)** without copying a single byte into userland memory!

### Step 5: VFS, Page Cache & Dirty Pages
1. The data read from `source.txt` is loaded into the **Page Cache** (RAM pages).
2. The write operation does **not** write to physical disk immediately! The kernel writes the data into RAM pages allocated for `destination.txt` and flags them as **dirty pages**.
3. The system call returns immediately with success. This is why copying small files is near-instantaneous in Linux.

### Step 6: Background Flusher & Storage Device Driver
1. Dedicated kernel threads (`kworker` / `kflushd`) periodically inspect dirty pages.
2. The kernel Block Layer groups adjacent sector writes to optimize I/O throughput.
3. The storage driver (e.g., `nvme.ko`) submits write command queues via **Direct Memory Access (DMA)** directly to the storage controller.
4. The NVMe controller burns the bits into physical NAND flash cells.
5. The storage device fires a hardware interrupt (**IRQ**) to inform the CPU that physical persistence is finished.

### Step 7: Closing & Termination
1. `cp` calls `close(fd_in)` and `close(fd_out)`.
2. `cp` calls `exit_group(0)`.
3. The kernel reclaims the process's virtual memory and file descriptor tables.
4. The parent shell wakes up, prints the prompt, and awaits your next command!
---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [04 - Utilities and User Applications](./04-Utilities-and-User-Applications.md) | [README](./README.md) | [06 - Practical Commands](./06-Practical-Commands.md) |
