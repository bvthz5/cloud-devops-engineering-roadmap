# 02 - Control Groups: cgroups v1 vs cgroups v2

> **Summary:** A comprehensive, sentence-by-sentence architectural guide to Linux Control Groups (cgroups), the evolution from v1 multi-hierarchies to v2 unified trees, resource governance controllers (CPU, Memory, I/O, PIDs), and Out-Of-Memory (OOM) handling.

---

## 🏛️ 1. What Are Control Groups (cgroups)?

In modern multi-tenant server environments, multiple containerized processes share the exact same hardware infrastructure (CPU cores, RAM memory chips, storage drives, and network adapters). If a single malfunctioning or malicious container encounters a memory leak or enters an infinite loop, it could easily consume 100% of the host machine's physical RAM and CPU, starving critical operating system processes and crashing the entire server.

While **Linux Namespaces** govern *visibility* (restricting what system resources a process can see), **Control Groups (cgroups)** govern *resource allocation* (metering, limiting, prioritizing, and isolating how much physical hardware a process is permitted to consume).

Control groups were originally invented at Google in 2006 by engineers Paul Menage and Rohit Seth under the project name "Process Containers". The technology was officially merged into mainline Linux kernel version 2.6.24 in 2008 and renamed to Control Groups (cgroups). Without cgroups, container virtualization as we know it today would be fundamentally impossible.

---

## ⚙️ 2. Architectural Evolution: cgroups v1 vs. cgroups v2

The original implementation (**cgroups v1**) served the industry for over a decade but suffered from profound structural flaws that created severe operational instability under heavy enterprise workloads. Linux kernel architect Tejun Heo completely redesigned the subsystem, introducing **cgroups v2** (unified hierarchy), which is now the default in all modern Linux distributions (such as Ubuntu 22.04+, Debian 11+, RHEL 9+, Fedora).

```text
┌────────────────────────────────────────────────────────────────────────┐
│                   CGROUPS v1 (LEGACY MULTI-HIERARCHY)                  │
└────────────────────────────────────────────────────────────────────────┘

 /sys/fs/cgroup/
 ├── cpu/       ──► [ Process Tree A ] (PID 1024 belongs here)
 ├── memory/    ──► [ Process Tree B ] (PID 1024 can belong to a completely
 │                                      different group here!)
 └── blkio/     ──► [ Process Tree C ]
 
 ⚠️ Fatal Flaw: The memory and block I/O controllers operated independently.
 When buffered writes were written to disk, the I/O subsystem could not determine
 which memory cgroup originated the write, leading to unkillable OOM deadlocks!

┌────────────────────────────────────────────────────────────────────────┐
│                   CGROUPS v2 (MODERN UNIFIED HIERARCHY)                │
└────────────────────────────────────────────────────────────────────────┘

 /sys/fs/cgroup/
 └── docker/
     └── <container-id>/
         ├── cgroup.controllers (cpu memory io pids)
         ├── cpu.max            (CFS bandwidth limit)
         ├── memory.max         (Hard OOM kill threshold)
         ├── memory.high        (Soft throttling threshold)
         ├── io.max             (Direct disk IOPS/bandwidth limits)
         └── pids.max           (Fork-bomb process count limit)

 ✅ Major Innovation: A single unified process tree where memory, CPU, I/O,
 and PIDs are tied to the exact same hierarchical group!
```

---

## 🔬 3. The Four Core cgroups v2 Controllers in Docker

Docker interfaces directly with cgroups v2 via `runc` and `systemd`, translating high-level CLI flags into precise kernel parameters:

### 3.1 Memory Controller (`memory`)
The memory controller tracks all anonymous memory, swap space, and page cache allocated by processes in the container:
- **`memory.max` (Hard Boundary):** Defines the absolute ceiling of RAM the container is permitted to use. If processes attempt to allocate memory beyond this boundary, the Linux kernel invokes the **Out-Of-Memory (OOM) Killer**, terminating the container with an **Exit Code 137**.
- **`memory.high` (Soft Throttling Boundary):** A revolutionary feature in cgroups v2. When a container exceeds `memory.high`, the kernel does not kill the process. Instead, it aggressively reclaims page cache and throttles the container's memory allocation speed, giving the application time to self-heal.
- **`memory.oom.group` (Atomic Container Killing):** In cgroups v1, the OOM killer often terminated only a single random child process, leaving the container in a corrupt, half-dead zombie state. Setting `memory.oom.group = 1` in cgroups v2 guarantees that if any process in the container triggers an OOM event, the kernel terminates **every process in the container simultaneously**.

### 3.2 CPU Controller (`cpu`)
The CPU controller implements the Linux kernel's **Completely Fair Scheduler (CFS)** bandwidth control:
- **`cpu.max`:** Configures CPU limits using a pair of values: `quota` and `period` (measured in microseconds).
  - The default period is `100000` microseconds (100 milliseconds).
  - If you configure Docker with `--cpus="1.5"`, Docker writes `150000 100000` into `cpu.max`. This grants the container processes up to 150 milliseconds of CPU execution time during every 100-millisecond scheduling window (equivalent to 1.5 dedicated CPU cores).
- **`cpu.weight`:** Configures proportional CPU priority (shares) on a scale from 1 to 10000 (defaulting to 100). When CPU cores are saturated, CPU cycles are divided proportionally according to weight.

### 3.3 Process Number Controller (`pids`)
The PIDs controller restricts the total number of processes and threads that can be instantiated within the cgroup:
- **`pids.max`:** Caps the maximum number of active PIDs.
- **Defense Against Fork Bombs:** A classic denial-of-service attack is a "fork bomb" (a script that spawns child processes in an infinite recursive loop, exhausting the Linux kernel process table and freezing the entire host). Setting `--pids-limit=200` ensures that even if a container executes a fork bomb, it crashes itself without affecting the host or other containers.

### 3.4 Block I/O Controller (`io`)
Controls disk read and write throughput and operations per second (IOPS):
- **`io.max`:** Enforces hard limits on read bytes per second (`rbps`), write bytes per second (`wbps`), read IOPS (`riops`), and write IOPS (`wiops`) tied to specific disk storage devices identified by their major and minor device numbers.

---

## 🛠️ 4. Docker CLI Flags to cgroups v2 Mapping

The table below illustrates how everyday Docker command-line flags map directly to specific cgroups v2 files on the host filesystem:

| Docker CLI Flag | Docker Compose Directive | cgroups v2 File Path | Kernel Behavioral Impact |
| :--- | :--- | :--- | :--- |
| `--memory="2g"` | `deploy.resources.limits.memory: 2048M` | `memory.max` | Hard RAM ceiling; triggers OOM 137 if breached. |
| `--memory-reservation="1g"` | `deploy.resources.reservations.memory: 1024M` | `memory.low` | Soft guaranteed memory buffer during system pressure. |
| `--cpus="2.0"` | `deploy.resources.limits.cpus: '2.0'` | `cpu.max` | Writes `200000 100000` (quota/period). |
| `--cpu-shares=512` | N/A (Proportional weighting) | `cpu.weight` | Proportional CPU weight relative to other workloads. |
| `--pids-limit=150` | `deploy.resources.limits.pids: 150` | `pids.max` | Halts `fork()` system calls beyond 150 processes. |

---

## 🔍 5. Inspecting cgroups v2 Directly on the Linux Host

DevOps engineers can verify kernel enforcement by reading the virtual `/sys/fs/cgroup/` filesystem directly from the host operating system:

```bash
# 1. Identify the container's full 64-character ID
CONTAINER_ID=$(docker inspect -f '{{.Id}}' my_running_container)

# 2. Inspect active cgroup v2 limits set for that container:
cat /sys/fs/cgroup/docker/${CONTAINER_ID}/memory.max
cat /sys/fs/cgroup/docker/${CONTAINER_ID}/cpu.max
cat /sys/fs/cgroup/docker/${CONTAINER_ID}/pids.max

# 3. View live, real-time memory consumption:
cat /sys/fs/cgroup/docker/${CONTAINER_ID}/memory.current

# 4. Monitor host-level cgroup resource utilization in real time:
sudo systemd-cgtop
```

---

## 📋 6. Summary and Navigation

Control Groups form the bedrock of container resource isolation, guaranteeing fair sharing of physical hardware and preventing single processes from crashing host infrastructure. By understanding the unified architecture of cgroups v2, CFS scheduler limits, memory throttle boundaries, and PID caps, engineers deploy multi-tenant container platforms with rock-solid predictability.

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 01 - Linux Kernel Namespaces Deep Dive](./01-Linux-Kernel-Namespaces-Deep-Dive.md) | [Index](../../../README.md) | [03 - Filesystem Isolation chroot to pivot root →](./03-Filesystem-Isolation-chroot-to-pivot-root.md) |
