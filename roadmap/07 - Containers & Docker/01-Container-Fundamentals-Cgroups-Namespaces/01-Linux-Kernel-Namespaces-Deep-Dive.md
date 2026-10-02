# 01 - Linux Kernel Namespaces Deep Dive

## 1. What Are Linux Namespaces?

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

## 2. Inspecting a Process's Namespaces

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
