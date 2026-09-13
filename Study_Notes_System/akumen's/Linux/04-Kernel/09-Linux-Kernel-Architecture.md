# 09 - Linux Kernel Architecture & Evolution

The internal codebase of the Linux kernel is structured into modular layers that balance raw execution speed with cross-platform hardware portability.

---

## 1. Architectural Style: Modular Monolithic

Computer architecture defines several paradigms for operating system kernels:

```
Monolithic (Linux)           Microkernel (seL4)           Hybrid (Windows/macOS)
┌──────────────────────┐    ┌──────────────────────┐     ┌──────────────────────┐
│ VFS │ Net │ Sched │MM│    │ IPC │ Basic Sched    │     │ VFS │ Net │ Sched │MM│
│ Drivers │ Filesystems│    │ (Drivers in User)    │     │ (Display/Driver mix) │
└──────────────────────┘    └──────────────────────┘     └──────────────────────┘
All in Ring 0 (Fastest)     Minimal in Ring 0 (Safe)     Pragmatic compromise
```

### Why Linux Chose Modular Monolithic:
1. **Raw Performance:** Subsystems (VFS, memory management, networking) communicate via direct inline C function calls. There are no Inter-Process Communication (IPC) context switches between kernel components.
2. **Modularity without Speed Loss:** By compiling device drivers and optional filesystems as **Loadable Kernel Modules (LKMs)**, the kernel remains lightweight while retaining the flexibility to load code on demand.

---

## 2. Directory Structure of the Linux Kernel Source Code

When you clone the official Linux source repository (`torvalds/linux`), the directory tree reveals its clean separation of concerns:

```text
linux/
├── arch/            # Architecture-dependent code (x86, arm64, riscv, s390)
│   ├── x86/         # Page tables, context switching assembly for Intel/AMD
│   └── arm64/       # Architecture code for Apple Silicon, AWS Graviton
├── kernel/          # Core scheduler, locking (spinlocks/mutexes), timers
├── mm/              # Memory management (Buddy allocator, SLUB, paging, swap)
├── fs/              # Virtual Filesystem & filesystem drivers (ext4, xfs, btrfs)
├── net/             # Network stack (IPv4, IPv6, TCP, UDP, Netfilter, eBPF)
├── drivers/         # Millions of lines of hardware peripheral drivers
├── include/         # Core C header files
└── init/            # Kernel initialization and early boot code
```

---

## 3. The Modern Revolution: eBPF (Extended Berkeley Packet Filter)

Historically, if a cloud engineer wanted to extend kernel functionality, they had two choices:
1. Convince Linus Torvalds and the community to merge code into the official kernel (taking years).
2. Write a custom out-of-tree **Kernel Module (`.ko`)** (risking fatal kernel panics if a pointer bug existed).

### The eBPF Paradigm Shift:
**eBPF** enables developers to write sandboxed, event-driven bytecode that runs directly inside the Linux kernel at runtime without modifying kernel source code or loading untrusted modules.

```
User Space (Go / C):
Writes monitoring, security, or networking logic (e.g. Cilium / Falco)
         │
         ▼ Compiles to eBPF Bytecode via Clang/LLVM
Loads into kernel via `bpf()` system call
═════════════════════════════════════════════════════════════
Linux Kernel (Ring 0):
         │
         ▼
┌─────────────────────────────────────────────────────────────┐
│ eBPF In-Kernel Verifier                                     │
│ • Statically proves program cannot crash the kernel         │
│ • Proves all memory accesses are bounded and in-bounds      │
│ • Proves the program terminates (no infinite loops)         │
└───────────────────────────┬─────────────────────────────────┘
                            │ Verified Safe!
                            ▼
┌─────────────────────────────────────────────────────────────┐
│ In-Kernel JIT (Just-In-Time) Compiler                       │
│ Compiles bytecode directly to native machine instructions   │
└───────────────────────────┬─────────────────────────────────┘
                            │ Attached to:
                            ▼
[ Network Packets (XDP) | Syscall Entry | Kprobes | Tracepoints ]
```

eBPF is now the foundational engine behind modern Cloud-Native networking (Cilium), container security (Falco), and zero-overhead observability (Pixie, BCC).
