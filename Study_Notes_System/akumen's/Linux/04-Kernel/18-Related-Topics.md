# 18 - Related Topics & Advanced Kernel Engineering

Understanding kernel architecture positions you to master container runtimes, high-performance networking, and cloud security frameworks.

---

## 🗺️ Downstream Engineering Roadmap

```
                          ┌──────────────────────────────────────┐
                          │   04 - The Linux Kernel (Current)    │
                          └──────────────────┬───────────────────┘
                                             │
             ┌───────────────────────────────┼───────────────────────────────┐
             ▼                               ▼                               ▼
┌─────────────────────────┐     ┌─────────────────────────┐     ┌─────────────────────────┐
│ 05 - eBPF & Observa-    │     │ 06 - Container Runtimes │     │ 07 - Linux Security     │
│      bility Engine      │     │ • OCI, runc, containerd │     │      Modules (LSM)      │
│ • bpftrace & BCC tools  │     │ • Cgroups v2 hierarchy  │     │ • SELinux / AppArmor    │
│ • Kernel kprobes/XDP    │     │ • Mount & PID namespaces│     │ • Seccomp syscall filter│
└────────────┬────────────┘     └────────────┬────────────┘     └────────────┬────────────┘
             │                               │                               │
             └───────────────────────────────┼───────────────────────────────┘
                                             ▼
                                ┌─────────────────────────┐
                                │ 08 - Linux Networking   │
                                │ • Netfilter / iptables  │
                                │ • Sockets & TCP tuning  │
                                │ • Bridge & veth pairs   │
                                └─────────────────────────┘
```

---

## 🔗 Deeply Connected Specializations

### 1. Linux Security Modules (LSM) & Seccomp
- **Why it connects:** The kernel provides hook points through the LSM framework allowing security engines to inspect and authorize operations before they execute.
- **Key Concepts:** **SELinux** (Mandatory Access Control using security contexts), **AppArmor** (path-based profile enforcement), and **Seccomp (Secure Computing Mode)**, which restricts the specific system calls a containerized process is allowed to execute.

### 2. Container Runtime Architecture (`runc` & OCI)
- **Why it connects:** Container engines like Docker and Podman delegate the direct invocation of kernel namespaces and cgroups to an OCI runtime (such as `runc`).
- **Key Concepts:** `CLONE_NEW*` system call flags, pivot_root, cgroups v2 memory controller (`memory.max`), and rootless containers via User namespaces.

### 3. Kernel Performance Profiling (`perf` & eBPF)
- **Why it connects:** Pinpointing CPU bottlenecks in high-scale cloud services requires profiling CPU instruction cycles directly inside the kernel.
- **Key Concepts:** Hardware performance counters (PMU), CPU cache misses, flame graphs, `perf top`, and `bpftrace`.

---

## 📚 Authoritative Kernel References
1. **Robert Love:** *Linux Kernel Development* (3rd Edition) — The canonical guide to kernel subsystems, schedulers, and memory allocation.
2. **Jonathan Corbet, Alessandro Rubini, Greg Kroah-Hartman:** *Linux Device Drivers* (LDD3) — Comprehensive manual on writing kernel drivers.
3. **Official Linux Kernel Documentation:** [docs.kernel.org](https://docs.kernel.org/) — The upstream documentation maintained by kernel developers.
---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [17 - Quick Revision](./17-Quick-Revision.md) | [README](./README.md) | — |
