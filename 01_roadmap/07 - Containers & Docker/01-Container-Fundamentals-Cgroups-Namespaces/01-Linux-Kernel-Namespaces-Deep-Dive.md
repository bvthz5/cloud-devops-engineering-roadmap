# 01 - Linux Kernel Namespaces Deep Dive

## 1. History of Linux Process Isolation & Containers

| Technology / Milestone | Inventor(s) & Organization | Year | The Real Problem / Crisis That Triggered It | Real-World Impact |
| :--- | :--- | :--- | :--- | :--- |
| **`chroot` (Change Root)** | **Bill Joy** (Unix V7 / BSD) | **1979** | During Unix development, running new build and test scripts risked overwriting critical system binaries in `/bin` and `/etc`. Joy added `chroot` to confine a process to a sub-tree of the filesystem. | The earliest ancestor of filesystem isolation; used for early build sandboxes. |
| **FreeBSD Jails** | **Poul-Henning Kamp** (FreeBSD) | **2000** | Shared web hosting providers had hundreds of mutual-untrusted customers on one server. If one customer's script was exploited, attackers could view other tenants' files. Jails partitioned files, processes, and IPs. | Established OS-level virtualization concepts for secure multi-tenant hosting. |
| **Linux Control Groups (`cgroups`)** | **Paul Menage & Rohit Seth** (Google) | **2006–2007** | A runaway calculation batch job on a shared Google server could consume 100% of CPU and RAM, starving critical search/Gmail services. Google built `cgroups` to strictly enforce CPU, RAM, and I/O quotas. | Provided the resource governor engine essential for all modern containers. |
| **Linux Namespaces** | **Al Viro, Eric Biederman et al.** (Linux Kernel Community) | **2002–2006** | Linux processes had a shared global view of PIDs, network interfaces, and mounts. Kernel developers introduced namespaces to give processes their own private, virtualized system views. | The architectural pillar that makes a container feel like a dedicated virtual server. |
| **The Docker Revolution** | **Solomon Hykes, Sebastien Pahl** (dotCloud / Docker) | **2013** | **"Works on My Machine"**: Mismatched libraries (`libc`, Python, OpenSSL) between dev laptops and prod servers caused deployment hell. Hykes packaged code, dependencies, and rootfs into immutable layered images (`Dockerfile` + UnionFS). | Standardized containerization across the entire global IT and cloud industry. |

### 🕰️ Visual Timeline: The Evolution of Containers

```text
 1979 ────── 2000 ────── 2002/2006 ─── 2006/2007 ── 2008 ────── 2013 ────── 2015 ───── Present
   │           │             │             │           │           │           │           │
 chroot     FreeBSD        Linux         cgroups      LXC        Docker       OCI &      Kubernetes
Bill Joy     Jails      Namespaces      Google     User-Space   Solomon      runc       Cloud-Native
Unix V7    Kamp / ISP   Process View    Resource   CLI Tools    Hykes      Standard     Enterprise
Sandboxing Multi-Tenant Virtualization  Quotas     Linux Cont.  Images     Spec         Standard
```

---

## 2. What Are Linux Namespaces?

Namespaces are a feature of the Linux kernel that partitions kernel resources such that one set of processes sees one set of resources while another set of processes sees a completely different, isolated set of resources.

```text
+-----------------------------------------------------------------------------------+
|                        The 7 Linux Kernel Namespaces                              |
+-------------------+-----------------------+---------------------------------------+
| Namespace         | Kernel Flag           | What It Isolates                      |
+-------------------+-----------------------+---------------------------------------+
| **PID**           | `CLONE_NEWPID`        | Process IDs (Process inside is PID 1) |
| **NET**           | `CLONE_NEWNET`        | Network devices, IP routing, iptables |
| **MNT**           | `CLONE_NEWNS`         | Filesystem mount points and disks     |
| **IPC**           | `CLONE_NEWIPC`        | System V IPC, POSIX message queues    |
| **UTS**           | `CLONE_NEWUTS`        | Hostname and NIS domain name          |
| **USER**          | `CLONE_NEWUSER`       | User and Group UIDs/GIDs mappings     |
| **CGROUP**        | `CLONE_NEWCGROUP`     | Cgroup root directory view            |
+-------------------+-----------------------+---------------------------------------+
```

---

## 3. Inspecting a Process's Namespaces

Every Linux process exposes symlinks to its active namespaces inside `/proc/<PID>/ns/`:

```bash
# View namespaces of current shell
ls -la /proc/$$/ns/

# Inspect namespaces of a running container process (e.g. PID 4321)
ls -la /proc/4321/ns/
```

### System Calls Controlling Namespaces
1. **`clone(..., flags)`**: Spawns a new child process with new isolated namespaces specified by bitwise OR flags (`CLONE_NEWPID | CLONE_NEWNET`).
2. **`unshare(flags)`**: Disassociates the calling process from its current namespaces and moves it into newly created ones.
3. **`setns(fd, nstype)`**: Attaches the calling thread to an existing namespace (the foundation of `docker exec`).

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← Prev Section (06 - Web Servers & Reverse Proxies)](../../06%20-%20Web%20Servers%20%26%20Reverse%20Proxies/10-Web-Security-WAF-and-Hardening/12-Quick-Revision.md) | [Index](../../../README.md) | [02 - Control Groups cgroups v1 vs cgroups v2 →](./02-Control-Groups-cgroups-v1-vs-cgroups-v2.md) |
