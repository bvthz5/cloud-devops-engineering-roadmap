# Operating System and Kernel Fundamentals

Welcome to the definitive guide on **Operating System and Kernel Fundamentals** for Cloud and DevOps Engineers. The Linux kernel forms the bedrock upon which container runtimes, Kubernetes orchestrators, virtualization hypervisors, and cloud infrastructure operate. Understanding kernel internals allows you to master debugging, resource tuning, container security, and high-performance system design.

---

## Complete Topic Syllabus & Roadmap Mapping

Below is the complete, structured index of all **90 fundamental topics** plus **modern container and kernel technologies (cgroups v2, eBPF, seccomp)** covered in this module:

### 1. Architecture, Modes, and Kernel Models (Topics 1–12)
1. Operating System Fundamentals
2. Operating System Architecture
3. OS Responsibilities
4. Kernel
5. Kernel Architecture
6. Monolithic Kernel (Linux)
7. Microkernel (Mach, seL4, QNX)
8. Hybrid Kernel (Windows NT, macOS XNU)
9. User Space
10. Kernel Space
11. Privileged Mode (Ring 0)
12. User Mode (Ring 3)

### 2. System Calls & Kernel Traps (Topics 13–15)
13. System Calls
14. System Call Interface (libc wrapper, sysenter, syscall assembly trap)
15. System Call Categories (Process Control, File Management, Device Management, Information Maintenance, Communication)

### 3. Processes & Lifecycle Management (Topics 16–29)
16. Process (Process Control Block - `task_struct`)
17. Process Lifecycle
18. Process ID (PID)
19. Parent Process (PPID)
20. Child Process
21. Process Tree (`pstree`)
22. Process States (Running `R`, Sleeping `S`/`D`, Stopped `T`, Zombie `Z`)
23. Process Creation
24. `fork()`
25. `exec()` / `execve()`
26. `wait()` / `waitpid()`
27. Process Termination (`exit()`)
28. Zombie Process (Defunct, cleanup via parent `wait()`)
29. Orphan Process (Re-parenting to PID 1 / subreaper)

### 4. Threads & CPU Scheduling (Topics 30–39)
30. Thread (Lightweight Process - LWP)
31. Process vs Thread
32. Multithreading (Concurrency vs Parallelism)
33. CPU Scheduling
34. Scheduler (Linux Completely Fair Scheduler - CFS / EEVDF)
35. Scheduling Algorithms (FIFO, Round Robin, Priority, CFS)
36. Preemptive Scheduling
37. CPU Time Slice (Latency & Granularity)
38. Context Switching (Voluntary vs Involuntary)
39. CPU Affinity (`taskset`, pinning, isolation)

### 5. Memory Management & Paging Subsystem (Topics 40–49)
40. Memory Management
41. Virtual Memory
42. Physical Memory
43. Memory Allocation (`brk`, `sbrk`, `mmap`, slab/slub allocator)
44. Memory Protection (NX bit, ASLR, Page table permissions)
45. Paging
46. Page Tables (Multi-level page tables, CR3 register)
47. Page Faults (Minor, Major, Invalid / SIGSEGV)
48. Swapping & Swappiness (`vm.swappiness`)
49. Memory-Mapped Files (`mmap()`)

### 6. Filesystems, Inodes, & File Descriptors (Topics 50–60)
50. Filesystem (VFS - Virtual File System)
51. Filesystem Hierarchy (FHS)
52. File (Regular, Directory, Link, Device, Socket, FIFO)
53. Directory (Directory Entry - `dentry`)
54. File Metadata (`stat`)
55. Inode (Index Node, metadata storage, data blocks, link counts)
56. File Descriptor (Integer handles)
57. Standard Input (`stdin` - FD 0)
58. Standard Output (`stdout` - FD 1)
59. Standard Error (`stderr` - FD 2)
60. File Descriptor Table (`/proc/[PID]/fd/`)

### 7. Signals & Inter-Process Communication (IPC) (Topics 61–70)
61. Pipes (Anonymous Pipes `|`)
62. Signals (Asynchronous kernel interrupts)
63. Signal Handling (`SIGINT`, `SIGTERM`, `SIGKILL`, `SIGHUP`, `SIGCHLD`)
64. Inter-Process Communication (IPC)
65. Pipes (Uni-directional communication)
66. Named Pipes (FIFOs - `mkfifo`)
67. Unix Domain Sockets (UDS - Stream and Datagram IPC)
68. Shared Memory (`shmget`, `shmat`, POSIX `/dev/shm`)
69. Message Queues (POSIX & System V IPC)
70. Semaphores (Mutexes, counting semaphores, synchronization)

### 8. System Environment, Daemons, & Drivers (Topics 71–76)
71. Environment Variables (`export`, `/proc/[PID]/environ`)
72. Shell (Command interpreter)
73. Daemon (Background process detached from controlling terminal)
74. Service (Managed background unit)
75. Device Driver (Kernel-to-hardware translator)
76. Kernel Module (`lsmod`, `modprobe`, `insmod`, `rmmod`)

### 9. Kernel Networking, Sockets, & Security (Topics 77–85)
77. Kernel Networking (Netfilter, TCP/IP stack, socket buffers `sk_buff`)
78. Network Sockets (`socket()`, `bind()`, `listen()`, `accept()`, `connect()`)
79. Kernel Security (LSM - SELinux, AppArmor)
80. Users and Groups (UID, GID, effective UID)
81. File Permissions (rwx, octal, SUID, SGID, Sticky bit)
82. Access Control (POSIX ACLs - `setfacl`, `getfacl`)
83. Capabilities (`cap_net_bind_service`, `cap_sys_admin`, etc.)
84. Namespaces (Mount, PID, Net, IPC, UTS, User, Cgroup)
85. Resource Control (Control Groups - cgroups)

### 10. Boot, Init Systems, & Kernel Management (Topics 86–90)
86. OS Boot Process (BIOS/UEFI → Bootloader → Kernel → Init)
87. Init System (SysVinit, Upstart, systemd)
88. systemd (Units, Targets, `systemctl`, `journalctl`)
89. Kernel Parameters (`sysctl`, `/proc/sys/`)
90. Kernel Logs (`dmesg`, `/var/log/kern.log`)

### 11. Modern Container & Cloud Kernel Extensions (Essential DevOps Topics)
- **Control Groups v2 (cgroups v2):** Unified hierarchy, memory high vs max, CPU pressure metrics.
- **eBPF (Extended Berkeley Packet Filter):** In-kernel sandboxed programmability for tracing, observability, and networking (Cilium, Falco, bpftrace).
- **Seccomp (Secure Computing Mode):** Restricting process syscalls in Docker/Kubernetes.

---

## File Structure in this Module

- [`01-OS-Architecture-Kernel-Types-and-Dual-Mode.md`](./01-OS-Architecture-Kernel-Types-and-Dual-Mode.md)
- [`02-System-Calls-Interface-and-Categories.md`](./02-System-Calls-Interface-and-Categories.md)
- [`03-Process-Lifecycle-fork-exec-States-and-Zombies.md`](./03-Process-Lifecycle-fork-exec-States-and-Zombies.md)
- [`04-Threads-Multithreading-and-CPU-Scheduling.md`](./04-Threads-Multithreading-and-CPU-Scheduling.md)
- [`05-OS-Memory-Management-Paging-Swap-and-mmap.md`](./05-OS-Memory-Management-Paging-Swap-and-mmap.md)
- [`06-Filesystem-Architecture-Inodes-and-File-Descriptors.md`](./06-Filesystem-Architecture-Inodes-and-File-Descriptors.md)
- [`07-Signals-and-Inter-Process-Communication-IPC.md`](./07-Signals-and-Inter-Process-Communication-IPC.md)
- [`08-Environment-Variables-Daemons-and-Device-Drivers.md`](./08-Environment-Variables-Daemons-and-Device-Drivers.md)
- [`09-Kernel-Networking-Sockets-and-Security.md`](./09-Kernel-Networking-Sockets-and-Security.md)
- [`10-Init-Systems-systemd-and-Boot-Sequence.md`](./10-Init-Systems-systemd-and-Boot-Sequence.md)
- [`11-Kernel-Parameters-Sysctl-and-Kernel-Logs.md`](./11-Kernel-Parameters-Sysctl-and-Kernel-Logs.md)
- [`12-Modern-Kernel-Tech-cgroups-v2-eBPF-and-seccomp.md`](./12-Modern-Kernel-Tech-cgroups-v2-eBPF-and-seccomp.md)
- [`13-Real-World-Scenarios.md`](./13-Real-World-Scenarios.md)
- [`14-Troubleshooting.md`](./14-Troubleshooting.md)
- [`15-Interview-QA.md`](./15-Interview-QA.md)
- [`16-Hands-On-Practice.md`](./16-Hands-On-Practice.md)
- [`17-MCQ.md`](./17-MCQ.md)
- [`18-Quick-Revision.md`](./18-Quick-Revision.md)
