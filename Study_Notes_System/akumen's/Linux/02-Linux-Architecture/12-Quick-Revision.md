# 12 - Quick Revision & 5-Minute Architecture Cheat Sheet

Bookmark this page for rapid architectural review before interviews and on-call rotations.

---

## ⚡ The 5 Layers Summary

| Layer | Name | Space | Privilege | Core Responsibilities |
|---|---|---|---|---|
| **Layer 5** | **User Applications** | User Space | Ring 3 | Microservices, databases, Python runtimes, web servers. |
| **Layer 4** | **System Utilities** | User Space | Ring 3 | GNU Coreutils (`cp`, `ls`), shells (`bash`), `systemd`, `sshd`. |
| **Layer 3** | **System Libraries** | User Space | Ring 3 | `glibc` / `musl`: POSIX API wrappers, stdio buffering, `malloc`. |
| **—** | **Syscall Boundary** | **Boundary**| **Switch** | Assembly instruction: **`syscall`** (Ring 3 ➔ Ring 0 transition). |
| **Layer 2** | **Linux Kernel** | Kernel Space | Ring 0 | Process scheduler, virtual memory, VFS, networking, drivers. |
| **Layer 1** | **Physical Hardware** | Silicon | Hardware | CPU, MMU, RAM, NVMe/SATA storage, NICs, PCIe bus. |

---

## 💻 System Call Calling Convention (x86_64)

```
Register   Role
────────   ──────────────────────────────────────────────
%rax       System Call Number (e.g. 0=read, 1=write, 257=openat)
%rdi       Argument 1
%rsi       Argument 2
%rdx       Argument 3
%r10       Argument 4
%r8        Argument 5
%r9        Argument 6
Instruction: `syscall` executes kernel jump; `sysret` returns.
```

---

## 📋 The 7-Step `cp` File Copy Checklist

1. **User/Shell:** User presses enter; shell parses tokens and resolves `/usr/bin/cp` in `$PATH`.
2. **Process Creation:** Shell calls `fork()` to clone itself and `execve()` to launch the `cp` ELF binary.
3. **Dynamic Linking:** Linker (`ld-linux.so`) maps `libc.so.6` into the process address space.
4. **Opening Descriptors:** `cp` issues `openat()` for source (read-only ➔ FD 3) and destination (write/create ➔ FD 4).
5. **Data Transfer:** `cp` invokes `copy_file_range(3, ..., 4, ...)` for in-kernel zero-copy (or a `read`/`write` loop).
6. **Kernel & Page Cache:** VFS maps inodes; kernel writes blocks into RAM **Page Cache** as "dirty pages"; storage driver writes to SSD via DMA.
7. **Cleanup:** `cp` calls `close(3)`, `close(4)`, and `exit_group(0)`. Shell wakes up from `wait4()`.

---

## 🛠️ Architecture Command Quick-Reference

```bash
# 1. Trace system calls live:
strace -e trace=openat,read,write,close <command>

# 2. Syscall performance profiling:
strace -c <command>

# 3. Inspect dynamic library dependencies:
ldd /usr/bin/cp

# 4. View kernel release and architecture:
uname -a

# 5. List loaded kernel modules:
lsmod

# 6. Load / unload kernel module:
sudo modprobe <module_name>
sudo modprobe -r <module_name>

# 7. Monitor context switches & CPU modes:
vmstat 1

# 8. Check kernel ring buffer for hardware/OOM errors:
sudo dmesg -T --level=err,warn
```
---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [11 - MCQ](./11-MCQ.md) | [README](./README.md) | [13 - Related Topics](./13-Related-Topics.md) |
