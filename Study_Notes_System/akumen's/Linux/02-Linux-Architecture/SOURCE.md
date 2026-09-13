# Curriculum Mapping & Source Attribution

This module is based on the **Linux Architecture** curriculum, structured and expanded for production Cloud & DevOps engineering.

---

## 📖 Source-Derived Core Concepts (From "Linux Architecture.pdf")

The following foundational elements form the direct core of the curriculum:
1. **The 5-Layer Model:**
   - Layer 1: Hardware
   - Layer 2: Linux Kernel
   - Layer 3: System Libraries (`glibc`)
   - Layer 4: System Utilities & Shell
   - Layer 5: User Applications
2. **User Space vs. Kernel Space:**
   - Unprivileged application mode vs. privileged supervisor mode.
   - The role of CPU protection rings.
3. **The `cp` (File Copy) Architectural Trace:**
   - Keystroke interpretation by the shell.
   - Fork and execve of `/usr/bin/cp`.
   - Dynamic library linkage to `libc.so.6`.
   - The `open()`, `read()`, `write()`, and `close()` system call flow.
   - File descriptors 0, 1, 2, 3, and 4.
   - Kernel responsibilities: process tracking, virtual memory allocation, VFS mapping, and storage device driver commands.

---

## 🚀 Extended Production Additions (Beyond the PDF)

To ensure full readiness for enterprise DevOps, SRE, and cloud engineering roles, this module includes the following extensions:
1. **Modern System Call Mechanics:**
   - Modern x86_64 register calling convention (`%rax`, `%rdi`, `%rsi`, etc.).
   - Modern kernel-space zero-copy optimization: **`copy_file_range`**.
2. **Kernel Architectural Deep Dives:**
   - Monolithic vs. Microkernel architectural comparison and trade-offs.
   - Loadable Kernel Modules (LKMs): `lsmod`, `modprobe`, `modinfo`.
   - The **eBPF** runtime engine and modern kernel observability.
3. **Production DevOps Scenarios:**
   - Context switching bottlenecks in multi-threaded microservices.
   - Kubernetes Pod Exit Code 137 (OOM Killer via memory cgroups).
   - CPU Steal Time (`%st`) on multi-tenant cloud hypervisors (AWS EC2 / GCP).
4. **Diagnostic Tooling:**
   - Live system call tracing with `strace` and `ltrace`.
   - Profiling CPU modes and context switches with `vmstat` and `perf`.
   - Uninterruptible sleep (State `D`) triage via `/proc/[PID]/wchan` and `/proc/[PID]/stack`.
5. **Assessment & Practice:**
   - 10 Technical interview questions with answers.
   - 4 Step-by-step terminal labs.
   - 10 Multiple choice self-assessment questions.
   - 5-Minute pre-interview revision cheat sheet.
