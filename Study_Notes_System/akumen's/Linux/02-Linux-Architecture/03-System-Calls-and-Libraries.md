# 03 - System Calls & The Standard C Runtime Library (glibc)

User applications cannot execute privileged hardware operations directly. When a program needs to write to a disk, allocate physical memory, or send a network packet, it must request the kernel's assistance through a **System Call (syscall)**.

---

## 1. What is a System Call?

A **system call** is a controlled programmatic entry point that allows a user space program to request a service from the operating system kernel.

```
┌─────────────────────────────────────────────────────────────┐
│ Application Code (Python, Go, Node.js, C)                   │
│   e.g., file.write("Hello")                                 │
└──────────────────────────────┬──────────────────────────────┘
                               │ Calls standard C library function
                               ▼
┌─────────────────────────────────────────────────────────────┐
│ Layer 3: C Standard Library (glibc / musl)                   │
│   write(fd, buf, count) wrapper function                    │
│   1. Places argument values into CPU registers              │
│   2. Loads syscall number into %rax (e.g., 1 for write)     │
│   3. Executes CPU instruction: `syscall`                    │
└──────────────────────────────┬──────────────────────────────┘
                               │ CPU switches from Ring 3 to Ring 0
═══════════════════════════════╪═══════════════════════════════
                               ▼
┌─────────────────────────────────────────────────────────────┐
│ Layer 2: Linux Kernel Syscall Dispatcher                    │
│   - Reads %rax from sys_call_table                          │
│   - Invokes sys_write(fd, buf, count)                       │
│   - Executes VFS write -> Page Cache -> Driver              │
│   - Returns result in %rax and executes `sysret`            │
└─────────────────────────────────────────────────────────────┘
```

---

## 2. The Anatomy of Syscall Invocation (x86_64 Calling Convention)

On modern 64-bit x86 systems, system calls are invoked via hardware CPU registers:

1. **Syscall Number:** The integer identifying the requested function is loaded into register **`%rax`**.
2. **Arguments:** Up to six parameters are passed in specific registers:
   - Arg 1: **`%rdi`**
   - Arg 2: **`%rsi`**
   - Arg 3: **`%rdx`**
   - Arg 4: **`%r10`**
   - Arg 5: **`%r8`**
   - Arg 6: **`%r9`**
3. **Execution:** The CPU executes the assembly instruction **`syscall`**.
4. The CPU automatically saves the instruction pointer (`%rip`) and flags, switches to the kernel stack, and jumps to the kernel's system call entry handler.
5. Upon completion, the kernel executes **`sysret`** to return to user space.

### Common x86_64 Syscall Numbers:

| Syscall Name | `%rax` Number | Purpose |
|---|:---:|---|
| **`read`** | `0` | Read bytes from an open file descriptor. |
| **`write`** | `1` | Write bytes to an open file descriptor. |
| **`open` / `openat`** | `2` / `257` | Open a file and return an integer file descriptor (`fd`). |
| **`close`** | `3` | Close an open file descriptor and release references. |
| **`stat` / `newfstatat`**| `4` / `262` | Fetch file metadata from an inode. |
| **`mmap`** | `9` | Map files or anonymous physical memory into process address space. |
| **`brk`** | `12` | Adjust the top of the process data segment (heap growth). |
| **`fork` / `clone`** | `57` / `56` | Create a new child process or lightweight thread. |
| **`execve`** | `59` | Replace current process image with a new executable binary. |
| **`exit`** | `60` | Terminate the calling process and return status code. |

---

## 3. Why System Libraries (glibc & musl) Are Essential

Applications almost never write raw assembly instructions like `syscall`. Instead, they link against **System Libraries (Layer 3)**, principally the **GNU C Library (`glibc`)** or **`musl-libc`**.

### The 4 Major Roles of the C Standard Library:
1. **Convenient Wrappers:** Wraps register manipulation and assembly traps into clean C function calls (e.g. `open("file.txt", O_RDONLY)`).
2. **POSIX Compliance:** Ensures identical function signatures work across different CPU architectures (x86_64, ARM64, RISC-V) and Unix variants.
3. **User Space I/O Buffering (`stdio.h`):**
   - Calling the `write()` system call for every single character would destroy performance due to constant mode switching.
   - `glibc`'s `printf()` and `fwrite()` buffer data in user space memory until an 8 KB buffer is full, then flushes it with a single `write()` system call!
4. **Memory Allocation Engine:**
   - Functions like `malloc()` and `free()` are **not system calls**!
   - `malloc()` requests large memory chunks from the kernel using `brk()` or `mmap()`, then carves up and tracks smaller blocks in user space.

```
┌───────────────────────────────────────────────────────────┐
│ Alpine Linux vs. Ubuntu/Debian Containers:                │
│                                                           │
│ • Ubuntu / Debian / RHEL:                                 │
│   Use GNU C Library (`glibc` - ~10 MB+ footprint).       │
│   Full POSIX features, internationalization, high speed.  │
│                                                           │
│ • Alpine Linux:                                           │
│   Uses `musl-libc` (~600 KB footprint).                  │
│   Ultra-lightweight container base images (5 MB total).   │
└───────────────────────────────────────────────────────────┘
```

---

## 4. Return Codes & `errno` Handling

When a kernel system call completes:
- **On Success:** Returns a non-negative integer (e.g., number of bytes read/written, or a new file descriptor `fd >= 0`).
- **On Failure:** The kernel returns a negative error code (e.g., `-2` for File Not Found, `-13` for Permission Denied).

The C library interceptor checks for negative values:
1. Inverts the negative value to a positive integer.
2. Stores it in the thread-local **`errno`** global variable.
3. Returns **`-1`** to the calling program.

```c
// Example in C:
int fd = open("/etc/nonexistent", O_RDONLY);
if (fd == -1) {
    perror("Error opening file"); 
    // Prints: "Error opening file: No such file or directory" (ENOENT = 2)
}
```
