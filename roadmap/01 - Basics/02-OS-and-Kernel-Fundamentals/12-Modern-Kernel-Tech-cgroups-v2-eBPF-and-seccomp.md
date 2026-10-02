# 12 — Modern Kernel Technologies: cgroups v2, eBPF, Seccomp, and io_uring

---

## 1. Why Modern Kernel Technologies Matter for DevOps

The traditional Unix model (processes, files, pipes) was designed in the 1970s. Over the past decade, cloud-native infrastructure, Kubernetes, and container security drove massive innovations directly into the Linux kernel core:
1. **cgroups v2:** Solved the resource-accounting bugs and multiple hierarchies of cgroups v1.
2. **eBPF:** Turned the Linux kernel into a programmable runtime without modifying kernel source code.
3. **Seccomp:** Enabled granular filtering of system calls to prevent container escapes.
4. **io_uring:** Completely redefined Linux storage and network asynchronous I/O performance.

---

## 2. Control Groups v2 (cgroups v2)

In **cgroups v1**, each resource controller (CPU, Memory, blkio, pids) lived in its own isolated filesystem hierarchy (`/sys/fs/cgroup/cpu`, `/sys/fs/cgroup/memory`). This caused severe synchronization bugs—for example, the memory controller could not coordinate with the block I/O controller for buffered page cache writeback throttling.

**cgroups v2** introduces a **Single Unified Hierarchy** mounted at `/sys/fs/cgroup`:

```text
cgroups v1 (Multi-Hierarchy Spaghetti)        cgroups v2 (Unified Tree)
/sys/fs/cgroup/                               /sys/fs/cgroup/
  ├── cpu/containerA                            ├── kubepods.slice/
  ├── memory/containerA                         │    ├── pod1/
  └── blkio/containerA                          │    │    ├── cpu.max
                                                │    │    ├── memory.max
                                                │    │    └── io.max
                                                │    └── pod2/
```

### Key Breakthroughs in cgroups v2
1. **Memory Ceiling Levels:**
   - **`memory.max`:** Hard ceiling. Exceeding this triggers the OOM killer.
   - **`memory.high`:** Soft throttle ceiling. If reached, the kernel slows down the process's execution and forces memory reclaim, preventing sudden OOM crashes.
2. **Pressure Stall Information (PSI):**
   Exposes real-time metrics (`cpu.pressure`, `memory.pressure`, `io.pressure`) tracking the percentage of time tasks are stalled waiting for CPU, memory, or disk resources.
3. **Unified Page Cache Writeback Accounting:** Memory-buffered writes are properly charged to the originating container's I/O limit.

### Checking cgroups v2 Support
```bash
# Returns 'cgroup2fs' if running cgroups v2
stat -fc %T /sys/fs/cgroup
```

---

## 3. eBPF (Extended Berkeley Packet Filter)

> **WHAT IS eBPF?**  
> eBPF is a revolutionary kernel technology that allows developers to run sandboxed, high-performance programs directly inside the Linux kernel at runtime **without changing kernel source code or loading untrusted kernel modules**.

```text
+-------------------------------------------------------------+
|               User Space Tools (Cilium, Falco, bpftrace)    |
+-------------------------------------------------------------+
                              │
                              ▼ Compiles C/Rust to eBPF Bytecode
+-------------------------------------------------------------+
|                     LINUX KERNEL                            |
|                                                             |
|  1. In-Kernel Verifier (Ensures code cannot crash kernel)   |
|  2. JIT Compiler (Translates bytecode into native CPU asm)  |
|                                                             |
|  Hook Points:                                               |
|  ├── XDP / Network Packets (Drop DDoS at NIC level)         |
|  ├── System Calls (Intercept execve, open, connect)         |
|  ├── Kprobes / Uprobes (Kernel & user function tracing)     |
|  └── Tracepoints                                            |
|                                                             |
|  eBPF Maps (Fast in-memory key-value data shared with user)  |
+-------------------------------------------------------------+
```

### Why eBPF is Dominating Cloud-Native:
1. **High-Speed Networking (Cilium):** Replaces legacy `iptables` (which slows down when managing thousands of Kubernetes services) with O(1) eBPF packet routing.
2. **Runtime Security (Falco, Tetragon):** Intercepts malicious system calls (e.g., shell spawned inside a container) instantly inside the kernel before damage occurs.
3. **Zero-Overhead Observability (Pixie, Parca):** Profiles CPU, memory allocations, and network latency continuously without requiring code instrumentation or sidecars.

---

## 4. Seccomp (Secure Computing Mode)

A standard Linux container shares the host kernel. The Linux kernel provides ~450 system calls, but a typical microservice needs fewer than 60.

**Seccomp** is a kernel security facility that restricts which system calls a process is allowed to make.

```text
Container Process (Node.js App)
       │
       ▼ Calls getpid(), read(), write() ──► [ ALLOW ]
       │
       ▼ Calls reboot(), swapon(), ptrace() ─► [ SECCOMP BLOCKS ]
                                                Returns EPERM or terminates process (SIGSYS)
```

### Docker's Default Seccomp Profile
By default, Docker applies a built-in seccomp filter that blocks ~44 dangerous system calls, including:
- `reboot`: Prevents container from rebooting the physical host.
- `kexec_load`: Prevents loading a foreign kernel.
- `sys_chroot`: Restricts privilege escalation.
- `mount` / `umount`: Prevents altering host mount tables.

### Applying Seccomp in Kubernetes Pods
```yaml
apiVersion: v1
kind: Pod
metadata:
  name: hardened-pod
spec:
  securityContext:
    seccompProfile:
      type: RuntimeDefault # Enforces container runtime's default seccomp profile
  containers:
  - name: api
    image: my-app:v1
```

---

## 5. `io_uring`: Asynchronous I/O Revolution

Historically, Linux relied on **`epoll`** for network events and **`libaio`** for asynchronous disk I/O. However, each operation still required expensive system calls.

Introduced by Jens Axboe in Linux 5.1, **`io_uring`** provides two lockless ring buffers shared between user space and kernel space:

```text
User Space Process                               Linux Kernel
┌─────────────────────────┐               ┌─────────────────────────┐
│ Submission Queue (SQ)   ├──────────────►│ Reads queued requests   │
│ (Submits I/O operations)│               │ Performs async DMA/read │
└─────────────────────────┘               └───────────┬─────────────┘
                                                      │
┌─────────────────────────┐                           ▼
│ Completion Queue (CQ)   │◄──────────────────────────┘
│ (Reads completed events)│   Writes completion results
└─────────────────────────┘   (Zero Syscalls, Zero Context Switches!)
```

### Performance Impact
`io_uring` can achieve millions of IOPS per core, delivering up to **3x the throughput of epoll** with dramatically lower CPU utilization. It is rapidly being adopted in high-performance engines like Node.js, Tokio (Rust), and Netty (Java).

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [11 - Kernel Parameters Sysctl and Kernel Logs](./11-Kernel-Parameters-Sysctl-and-Kernel-Logs.md) | [README](./README.md) | [13 - Real World Scenarios](./13-Real-World-Scenarios.md) |
