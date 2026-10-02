# 01 - Container Fundamentals (Cgroups & Namespaces)

A "container" is not a physical or virtual machine; it is simply a standard Linux process isolated from other processes via **Linux Kernel Namespaces** and throttled in hardware usage via **Control Groups (cgroups)**. Understanding these kernel primitives is mandatory for diagnosing container resource starvation, breakout security vulnerabilities, and low-level networking in cloud-native platforms.

---

## Module Index

| # | Topic | Description |
|---|---|---|
| 01 | [Linux Kernel Namespaces Deep Dive](./01-Linux-Kernel-Namespaces-Deep-Dive.md) | The 7 namespaces (`pid`, `net`, `mnt`, `ipc`, `uts`, `user`, `cgroup`), `clone()`, `unshare()`. |
| 02 | [Control Groups: cgroups v1 vs cgroups v2](./02-Control-Groups-cgroups-v1-vs-cgroups-v2.md) | Resource controllers (CPU, memory, blkio, pids), unified hierarchy in v2, PSI metrics. |
| 03 | [Filesystem Isolation: chroot to pivot_root](./03-Filesystem-Isolation-chroot-to-pivot-root.md) | Jail breakouts with `chroot`, atomic root swap with `pivot_root`, mount namespaces. |
| 04 | [Union Filesystems & OverlayFS Internals](./04-Union-Filesystems-and-OverlayFS-Internals.md) | Copy-on-Write (CoW), lowerdir, upperdir, merged, workdir, whiteout files. |
| 05 | [Building a Container from Scratch in Bash/C](./05-Building-a-Container-from-Scratch-in-Bash.md) | Constructing a working container using `unshare`, `pivot_root`, and cgroup controllers. |
| 06 | [Container Security Boundaries & Kernel Surface](./06-Container-Security-Boundaries-and-Kernel-Surface.md) | Shared kernel risks, system calls, capabilities (`CAP_SYS_ADMIN`), container escapes. |
| 07 | [Real-World Scenarios](./07-Real-World-Scenarios.md) | Enterprise post-mortems: Fork bomb crashing Kube node, cgroups v1 memory metric misreport. |
| 08 | [Troubleshooting & Diagnostic Runbooks](./08-Troubleshooting.md) | Inspecting `/proc/<pid>/ns/`, inspecting `/sys/fs/cgroup/`, using `nsenter` to inject tools. |
| 09 | [Interview Questions & Architectural Scenarios](./09-Interview-QA.md) | 10 Senior SRE/DevOps interview scenarios on namespaces, cgroups v2, and container runtimes. |
| 10 | [Hands-On Practice Labs](./10-Hands-On-Practice.md) | Lab 1: Namespace isolation via `unshare`; Lab 2: Memory cgroup throttler; Lab 3: `nsenter` debugging. |
| 11 | [Multiple-Choice Assessment (MCQ)](./11-MCQ.md) | 10 technical scenario MCQs with deep architectural rationales. |
| 12 | [Quick-Revision & Enterprise Cheat Sheet](./12-Quick-Revision.md) | High-density reference for kernel flags, namespace commands, and cgroup filesystem paths. |

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [Master Index](../../00-Master-Index.md) | [README](./README.md) | [01 - Linux Namespaces](./01-Linux-Kernel-Namespaces-Deep-Dive.md) |
